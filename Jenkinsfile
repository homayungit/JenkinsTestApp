pipeline {
    agent any

    environment {
        DOTNET_VERSION = '8.0'  // Adjust this to match your .NET Core SDK version
        PUBLISH_DIR = "C:\\Jenkins\\workspace\\IDPService8002\\publish"  // Path to publish directory
        NETWORK_PATH = "\\\\192.168.3.12\\publish_root"  // Network path
        DRIVE_LETTER = "F:"
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
                    // Map the network drive
                    bat "net use ${DRIVE_LETTER} ${NETWORK_PATH} /persistent:no"
                    
                    // Deploy to existing IIS site
                    bat "xcopy /Y /S ${PUBLISH_DIR}\\* ${DRIVE_LETTER}\\IDPService8002\\"
                    
                    // Unmap the network drive
                    bat "net use ${DRIVE_LETTER} /delete"

                    // Start the IIS site
                    bat "iisreset /start"
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
