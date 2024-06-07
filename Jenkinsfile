pipeline {
    agent any

    stages {
        stage('Without Docker') {
            steps {
                // This will run in jenkins
                sh '''
                    echo "Hello without docker"
                    ls -la
                    touch not-in-container.txt
                '''
            }
        }
        
        stage('With Docker') {
            agent {
                // Need the docker plugin installed in jenkins
                docker {
                    image 'node:18-alpine'
                    
                    // This will share the workspace.
                    reuseNode true
                }
            }
            steps {
                // This will run inside docker
                sh '''
                    echo "Hello with docker"
                    ls -la
                    touch in-container.txt
                    npm --version
                '''
            }
        }
    }
}