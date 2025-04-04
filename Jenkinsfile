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
                 bat 'tar -cvf app.tar .'
            }
        }
                stage('Deploy to Azure') {
            steps {
                withCredentials([
                    string(credentialsId: 'azure-client-id', variable: 'AZURE_CLIENT_ID'),
                    string(credentialsId: 'azure-client-secret', variable: 'AZURE_CLIENT_SECRET'),
                    string(credentialsId: 'azure-tenant-id', variable: 'AZURE_TENANT_ID')
                ]) {
                    bat """
                    az login --service-principal -u "%AZURE_CLIENT_ID%" -p "%AZURE_CLIENT_SECRET%" --tenant "%AZURE_TENANT_ID%"
                    az webapp up --name myPythonApp --resource-group myResourceGroup --runtime "PYTHON:3.9" --src-path .
                    """
                }
            }
        }

    }
}
