pipeline {
    agent any

    environment {
        DOTNET_VERSION = '8.0'  // Adjust this to match your .NET Core SDK version
        PUBLISH_DIR = "C:\\Jenkins\\workspace\\IDPService8002\\publish"  // Path to publish directory
        LOCAL_DEPLOY_DIR = "C:\inetpub\wwwroot\IDPService8002"  // Local deployment directory
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Restore Dependencies') {
            steps {
                bat "dotnet restore"
            }
        }

        stage('Build') {
            steps {
                bat "dotnet build --configuration Release"
            }
        }

        stage('Publish') {
            steps {
                bat "dotnet publish --configuration Release --output ${PUBLISH_DIR}"
            }
        }

        stage('Deploy to IIS') {
            steps {
                script {
                    // Deploy to existing IIS site
                    bat "xcopy /Y /S ${PUBLISH_DIR}\\* ${LOCAL_DEPLOY_DIR}\\"

                    // Start the IIS site

                }
            }
        }
    }

    post {
        success {
            echo 'Deployment to IIS completed successfully!'
        }
        failure {
            echo 'Deployment to IIS failed.'
        }
    }
}
