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
                withCredentials([string(credentialsId: 'azure-service-principal-2', variable: 'AZURE_SP')]) {
                    script {
                        def parts = AZURE_SP.split(':')
                        def clientId = parts[0]
                        def clientSecret = parts[1]
                        def tenantId = parts[2]

                        bat """
                        az login --service-principal -u "${clientId}" -p "${clientSecret}" --tenant "${tenantId}"
                        az webapp up --name myPythonApp --resource-group myResourceGroup --runtime "PYTHON:3.9" --src-path .
                        """
                    }
                }
            }
        }
    }
}
