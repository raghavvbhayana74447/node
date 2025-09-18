pipeline{
    agent { label 'cloud-agent'}
    tools{
        nodejs "node"
    }

    stages{
        stage('pulling git repo'){
            steps{
                git url: 'https://github.com/raghavvbhayana74447/node.git',
                branch: 'main'
            }
        }
        stage('installing dependencies'){
            steps{
                sh '''
                npm install
                '''
            }
        }
        stage('test'){
            steps{
                sh '''
                npm test
                '''
            }
        }
        stage('deploying'){
            steps{
                sh '''
                node server.js
                '''
            }
        }
    }
}