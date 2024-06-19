pipeline {
    agent any

    environment {
        DOTNET_VERSION = '8.0'  // Adjust this to match your .NET Core SDK version
        PUBLISH_DIR = "C:\\Jenkins\\workspace\\IDPService8002\\publish"  // Path to publish directory
        NETWORK_PATH = "\\\\192.168.3.12\\publish_root"  // Network path \\192.168.3.12\publish_root\IDPService8002
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
                    // Stop the IIS site (if already running)
                    bat "iisreset /stop"

                    // Disconnect the drive if it is already in use
                    bat "net use ${DRIVE_LETTER} /delete /y"

                    // Map network drive
                    bat """
                        net use ${DRIVE_LETTER} ${NETWORK_PATH} /user:admin.homayun H%M@k87!hameem
                    """

                    // Conditional check and directory creation
                    bat """
                        if exist ${DRIVE_LETTER}\\IDPService8002 (
                            rmdir /S /Q ${DRIVE_LETTER}\\IDPService8002
                        )
                        mkdir ${DRIVE_LETTER}\\IDPService8002
                    """

                    // Deploy to existing IIS site
                    bat "xcopy /Y /S ${PUBLISH_DIR}\\* ${DRIVE_LETTER}\\IDPService8002\\"

                    // Unmap network drive
                    bat "net use ${DRIVE_LETTER} /delete /y"

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
