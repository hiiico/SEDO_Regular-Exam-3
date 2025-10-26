pipeline {
    agent any
    
    tools {
        dotnetsdk 'dotnet-sdk-8.0'
    }
    
    environment {
        DOTNET_CLI_TELEMETRY_OPTOUT = '1'
        DOTNET_NOLOGO = 'true'
    }
    
    triggers {
        pollSCM('* * * * *')  // Check every minute for changes
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Setup') {
            steps {
                sh 'mkdir -p TestResults'  // Ensure TestResults directory exists
            }
        }
        
        stage('Restore Dependencies') {
            steps {
                sh 'dotnet restore'
            }
        }
        
        stage('Build') {
            steps {
                sh 'dotnet build --configuration Release --no-restore'
            }
        }
        
        stage('Run Tests') {
            steps {
                script {
                    // Run tests and generate TRX files
                    sh 'dotnet test --configuration Release --no-build --verbosity normal --logger "trx;LogFileName=test-results.trx" --results-directory ./TestResults'
                    
                    // Install trx2junit converter if not already present
                    sh 'dotnet tool install --global trx2junit --version 2.0.0'
                    
                    // Convert TRX to JUnit format
                    sh 'trx2junit ./TestResults/*.trx'
                }
            }
            post {
                always {
                    // Archive JUnit formatted results (allow empty if conversion fails)
                    junit allowEmptyResults: true, testResults: '**/TestResults/*.xml'
                    
                    // Also archive TRX files for reference
                    archiveArtifacts '**/TestResults/*.trx'
                }
            }
        }
    }
    
    post {
        always {
            echo "Pipeline execution completed - ${currentBuild.currentResult}"
        }
        success {
            echo '✅ Pipeline succeeded!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}