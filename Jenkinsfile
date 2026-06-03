pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'd4d1e472-a358-4acb-af81-7b50c7e236c8'
    }

    stages {

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install
                    npm test
                '''
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install
                    npm install -g netlify-cli

                    echo "Deploying to Netlify..."
                '''
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true,
                  testResults: 'test-results/junit.xml'
        }
    }
}