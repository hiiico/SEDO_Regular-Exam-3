node {
    stage('Checkout') {
        checkout scm
    }
    
    stage('Setup .NET') {
        // Ensure .NET 8.0 SDK is available
        env.DOTNET_CLI_TELEMETRY_OPTOUT = '1'
        env.DOTNET_NOLOGO = 'true'
    }
    
    stage('Restore') {
        sh 'dotnet restore'
    }
    
    stage('Build') {
        sh 'dotnet build --configuration Release --no-restore'
    }
    
    stage('Test') {
        sh 'dotnet test --configuration Release --no-build --verbosity normal --logger "trx;LogFileName=test-results.trx"'
        
        // Publish test results
        junit '**/TestResults/*.trx'
    }
    
    stage('Publish') {
        if (env.BRANCH_NAME == 'main') {
            sh 'dotnet publish --configuration Release --output ./publish --no-build'
            archiveArtifacts artifacts: 'publish/**/*', fingerprint: true
        }
    }
    
    stage('Cleanup') {
        // Clean up workspace
        cleanWs()
    }
}