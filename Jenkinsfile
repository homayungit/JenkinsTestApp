pipeline {
    agent any
    
    environment {
        // Define any environment variables here
        // DOTNET_VERSION = '5.0' // Change this to your required .NET Core version
        DOTNET_CLI_HOME = "C:\\Program Files\\dotnet"
    }
    
    stages {
        stage('Checkout') {
            steps {  
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    bat "dotnet restore"
                    bat "dotnet build --configuration Release"
                }
            }
        }             
        
        stage('Test') {
            steps {
                script {
                    bat "dotnet test --no-restore --configuration Release"
                }
            }
        }
        
        stage('Publish') {
            steps {
                script {
                    bat 'dotnet publish --no-restore --configuration Release --output .\\publish'
                }
            }
        }
    }
    
    post {               
        success {
            echo 'The build was successful!'
        }       
    }
}
