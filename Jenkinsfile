pipeline {
    agent any

    stages {
        stage('Build') {
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
                cleanWS()
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
    }
}