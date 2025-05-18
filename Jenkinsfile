pipeline {
    agent any

    environment{
        NRTLIFY_SITE_ID = '13dbfaca-f398-48a1-bb2a-4e674d6207e1'
    }

    stages {
        stage('Build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
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

        stage('Deploy') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version 
                    echo "Deploying to production. SITE ID: $NETLIFY_SITE_ID"
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
