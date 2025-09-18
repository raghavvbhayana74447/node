pipeline {
    agent { label 'cloud-agent' }

    tools {
        nodejs "node"
    }

    environment {
        RESOURCE_GROUP = "RnD-RaghavRG"
        APP_NAME       = "mywebapp74447"
        LOCATION       = "eastus"         
    }

    stages {
        stage('Pulling Git Repo') {
            steps {
                git url: 'https://github.com/raghavvbhayana74447/node.git', branch: 'main'
            }
        }

        stage('Installing Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Deploy to Azure with CLI') {
            steps {
                withCredentials([
                    usernamePassword(credentialsId: 'AzureServicePrincipal',
                                     usernameVariable: 'CLIENT_ID',
                                     passwordVariable: 'CLIENT_SECRET'),
                    string(credentialsId: 'AzureTenant', variable: 'TENANT_ID'),
                    string(credentialsId: 'AzureSubscription', variable: 'SUBSCRIPTION_ID')
                ]) {
                    sh '''
                        echo "Logging into Azure..."
                        az login --service-principal -u $CLIENT_ID -p $CLIENT_SECRET --tenant $TENANT_ID
                        az account set --subscription $SUBSCRIPTION_ID

                        echo "Zipping app..."
                        zip -r app.zip * .[^.]* || true

                        echo "Deploying to Azure Web App..."
                        az webapp deployment source config-zip \
                          --resource-group $RESOURCE_GROUP \
                          --name $APP_NAME \
                          --src app.zip
                    '''
                }
            }
        }
    }


}
