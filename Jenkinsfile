pipeline{
    agent { label 'cloud-agent'}

    tools{
        nodejs "node"
    }

    stages{
        stage('pulling git repo'){
            steps{
                echo "pulling repo"
                git url: 'https://github.com/raghavvbhayana74447/node.git',
                branch: 'main'
            }
        }
        stage('installing dependencies'){
            steps{
                echo "installing dependencies"
                sh '''
                npm install
                '''
            }
        }
        stage('test'){
            steps{
                echo "running tests"
                sh '''
                npm test
                '''
            }
        }
        stage('deploying')
        {
            steps{
                echo "deploying on azure app service"
                withCredentials([string(credentialsId: 'AzureServicePrincipal', variable: 'logincreds')])
                {
                    azureWebappPublish azureCredentialsId : "$logincreds",
                        resourceGroup: "RnD-RaghavRG"
                        appName: "mywebapp74447"
                }
            }
        }
    }
}