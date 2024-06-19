pipeline {
    agent any
    
    environment {
        DOTNET_VERSION = '8.0'  // Adjust this to match your .NET Core SDK version
        MSBUILD_PATH = "C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\BuildTools\\MSBuild\\Current\\Bin\\MSBuild.exe"  // Path to MSBuild
        IIS_SITE_NAME = 'JenkinsTestApp'  // Replace with your IIS site name
        PUBLISH_DIR = "C:\\Jenkins\\workspace\\JenkinsTestApp\\publish"  // Path to publish directory
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
                    
                    // Remove existing site content
                    bat "Remove-WebSite -Name ${IIS_SITE_NAME} -Force"
                    
                    // Create new IIS site
                    bat "New-WebSite -Name ${IIS_SITE_NAME} -PhysicalPath ${PUBLISH_DIR} -Port 8050 -Force"
                    
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
