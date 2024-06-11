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
                //cleanWs()
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

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage('e2e Test') {
            agent {
                docker {
                    //image 'mcr.microsoft.com/playwright/python:v1.44.0-jammy'
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install serve
                    node_modules/.bin/serve/serve -s build &
                    sleep 10
                    npx playwright test
                '''
            }
        }
    }

    post {
        always {
            junit testResults: 'jest-results/junit.xml', skipPublishingChecks: true
        }
    }
}