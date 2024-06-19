pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                bat 'dotnet restore'
                bat 'dotnet build --configuration Release'
            }
        }
        stage('Test') {
            steps {
                bat 'dotnet test --no-restore --configuration Release'
            }
        }
        stage('Publish') {
            steps {
                bat 'dotnet publish --no-restore --configuration Release --output .\\publish'
            }
        }
        stage('Deploy') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'mytestapp', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                    script {
                        try {
                            powershell script: '''
                                Write-Host "Starting deployment..."
                                # Your PowerShell deployment script here
                                Write-Host "Deployment completed successfully."
                            ''', returnStatus: true, label: 'Deploying Application'
                        } catch (Exception e) {
                            echo "Deployment failed: ${e.getMessage()}"
                            currentBuild.result = 'FAILURE'
                        }
                    }
                }
            }
        }
    }
}
