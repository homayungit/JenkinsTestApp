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
        stage('Deploy') {
            steps {
                script {
                      withCredentials([usernamePassword(credentialsId: 'JenkinsTestApp', passwordVariable: 'CREDENTIAL_PASSWORD', usernameVariable: 'CREDENTIAL_USERNAME')]) {
                    powershell '''
                    
                    $credentials = New-Object System.Management.Automation.PSCredential($env:CREDENTIAL_USERNAME, (ConvertTo-SecureString $env:CREDENTIAL_PASSWORD -AsPlainText -Force))

                    
                    New-PSDrive -Name X -PSProvider FileSystem -Root "\\\\Homayun-IT\\JenkinsTestApp" -Persist -Credential $credentials

                    
                    Copy-Item -Path '.\\publish\\*' -Destination 'X:\' -Force

                    
                    Remove-PSDrive -Name X
                    '''
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
}
