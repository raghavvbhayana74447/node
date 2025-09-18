pipeline {
    agent { label 'cloud-agent' }

    tools {
        nodejs "node"
    }

    environment {
        RESOURCE_GROUP = "RnD-RaghavRG"
        APP_NAME       = "mywebapp74447"
        PLAN_NAME      = "ASP-RnDRaghavRG-b5a6"   
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

        stage('Setup Azure Web App') {
            steps {
                withAzureCredentials(credentials: 'AzureServicePrincipal') {
                    sh '''
                        echo "Logging into Azure..."
                        az login --service-principal \
                          -u $ClientId \
                          -p $ClientSecret \
                          --tenant $tenantId
                        az account set --subscription $subscriptionId

                        echo "Checking Web App..."
                        az webapp show --resource-group $RESOURCE_GROUP --name $APP_NAME || \
                        az webapp create \
                          --resource-group $RESOURCE_GROUP \
                          --plan $PLAN_NAME \
                          --name $APP_NAME \
                          --runtime "NODE|20-lts"
                    '''
                }
            }
        }

        stage('Deploy to Azure with CLI') {
            steps {
                withAzureCredentials(credentials: 'AzureServicePrincipal') {
                    sh '''
                        echo "Logging into Azure..."
                        az login --service-principal \
                          -u $ClientId \
                          -p $ClientSecret \
                          --tenant $tenantId
                        az account set --subscription $subscriptionId

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
