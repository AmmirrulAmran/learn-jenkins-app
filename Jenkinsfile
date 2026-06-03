pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'd4d1e472-a358-4acb-af81-7b50c7e236c8'
        NETLIFY_AUTH_TOKEN = 'nfp_kC8pfRrYqLX6MEv25uQpctJyrbWicFYZ1454'
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
                    npm test -- --watchAll=false
                '''
            }
        }

        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    npm install
                    npm run build
                    test -f build/index.html
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
                withCredentials([
                    string(credentialsId: 'netlify-token', variable: 'NETLIFY_AUTH_TOKEN')
                ]) {
                    sh '''
                        npm install
                        npm install netlify-cli

                        npx netlify deploy \
                            --prod \
                            --dir=build \
                            --site=$NETLIFY_SITE_ID \
                            --auth=$NETLIFY_AUTH_TOKEN
                    '''
                }
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