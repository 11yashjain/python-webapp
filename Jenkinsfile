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
                bat 'pip install -r requirements.txt'
            }
        }
        stage('Publish') {
            steps {
                bat 'powershell Compress-Archive -Path * -DestinationPath app.zip'
            }
        }
        stage('Deploy to Azure') {
            steps {
                withCredentials([azureServicePrincipal('azure-service-principal-2')]) {
                    bat "az login --service-principal -u %AZURE_CREDENTIALS_USR% -p %AZURE_CREDENTIALS_PSW% --tenant %AZURE_CREDENTIALS_TEN%"
                    bat "az webapp up --name myPythonApp --resource-group myResourceGroup --runtime PYTHON:3.9 --src-path ."
                }
            }
        }
    }
}
