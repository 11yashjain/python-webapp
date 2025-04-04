pipeline {
    agent any

    environment {
        AZURE_CREDENTIALS_ID = 'azure-service-principal-2'
        RESOURCE_GROUP = 'myResourceGroup'
        APP_SERVICE_NAME = 'myPythonApp'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/11yashjain/python-webapp.git'
            }
        }

        stage('Set Up Python Environment') {
    steps {
        bat '"C:\\Users\\yashj\\AppData\\Local\\Programs\\Python\\Python310\\python.exe" -m venv venv'
        bat '.\\venv\\Scripts\\activate && python -m pip install --upgrade pip'
        bat '.\\venv\\Scripts\\activate && python -m pip install -r requirements.txt'
        bat '.\\venv\\Scripts\\activate && python -m pip install pytest'
    }
}


        stage('Run Tests') {
            steps {
                bat '.\\venv\\Scripts\\activate && python -m pytest'
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([string(credentialsId: AZURE_CREDENTIALS_ID, variable: 'AZURE_SP')]) {
                    script {
                        def parts = AZURE_SP.tokenize(':')
                        def clientId = parts[0]
                        def clientSecret = parts[1]
                        def tenantId = parts[2]
                        def subscriptionId = parts[3]

                        bat """
                        az login --service-principal -u "${clientId}" -p "${clientSecret}" --tenant "${tenantId}"
                        az account set --subscription "${subscriptionId}"

                        if exist publish (rmdir /s /q publish)
                        mkdir publish

                        :: Copy .py files and requirements.txt to publish folder
                        for %%f in (*.py) do copy "%%f" publish\\
                        if exist requirements.txt copy requirements.txt publish\\

                        powershell Compress-Archive -Path ./publish/* -DestinationPath ./publish.zip -Force
                        az webapp deployment source config-zip --resource-group "${RESOURCE_GROUP}" --name "${APP_SERVICE_NAME}" --src publish.zip
                        """
                    }
                }
            }
        }
    }

    post {
        failure {
            echo 'Deployment Failed!'
        }
        success {
            echo 'Deployment Successful!'
        }
    }
}
