pipeline {
    agent any

    stages {
        stage('Deploy') {

            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                
                sh'''
                    npm install netlify-cli 
                    netlify --version

                '''
            }
        }


        stage('Test'){

            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    test -f build/index.html
                    npm test
                    
                    
                    '''
            }
        }
        
        
    }

    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
}