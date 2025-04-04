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
                sh 'pip install -r requirements.txt'
            }
        }
        stage('Publish') {
            steps {
                sh 'zip -r app.zip .'
            }
        }
        stage('Deploy to Azure') {
            steps {
                withCredentials([azureServicePrincipal('azure-service-principal-2')]) {
                    sh 'az login --service-principal -u $AZURE_CREDENTIALS_USR -p $AZURE_CREDENTIALS_PSW --tenant $AZURE_CREDENTIALS_TEN'
                    sh 'az webapp up --name myPythonApp --resource-group myResourceGroup --runtime "PYTHON:3.9" --src-path .'
                }
            }
        }
    }
}
