pipeline {
    agent any
    environment {
        AZURE_CREDENTIALS = credentials('azure-service-principal-2')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/11yashjain/python-webapp.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    if (isUnix()) {
                        // Run shell commands if on a Unix-based system (Linux/macOS)
                        sh 'echo "Building the project on a Unix system"'
                    } else {
                        // Run batch commands if on Windows
                        bat 'echo "Building the project on a Windows system"'
                    }
                }
            }
        }

        stage('Publish') {
            steps {
                script {
                    if (isUnix()) {
                        // Example Unix command
                        sh 'echo "Publishing on Unix"'
                    } else {
                        // Example Windows command
                        bat 'echo "Publishing on Windows"'
                    }
                }
            }
        }

        stage('Deploy to Azure') {
            steps {
                script {
                    if (isUnix()) {
                        // Example Unix command for deployment
                        sh 'echo "Deploying to Azure from Unix"'
                    } else {
                        // Example Windows command for deployment
                        bat 'echo "Deploying to Azure from Windows"'
                    }
                }
            }
        }
    }
}
