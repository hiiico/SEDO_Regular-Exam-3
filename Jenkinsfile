pipeline {
    agent any
    
    triggers {
        pollSCM('* * * * *')
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Verify .NET') {
            steps {
                sh 'dotnet --version || echo ".NET not found, will install in next stage"'
            }
        }
        
        stage('Setup .NET') {
            when {
                expression { 
                    return sh(script: 'command -v dotnet', returnStatus: true) != 0 
                }
            }
            steps {
                sh '''
                    wget https://dot.net/v1/dotnet-install.sh -O dotnet-install.sh
                    chmod +x dotnet-install.sh
                    ./dotnet-install.sh --version 8.0.100
                    export PATH="$HOME/.dotnet:$PATH"
                '''
            }
        }
        
        stage('Restore dependencies') {
            steps {
                sh 'dotnet restore'
            }
        }
        
        stage('Build') {
            steps {
                sh 'dotnet build --no-restore'
            }
        }
        
        stage('Run UI Tests') {
            steps {
                sh 'dotnet test --no-build --verbosity normal'
            }
        }
    }
}