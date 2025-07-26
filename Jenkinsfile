pipeline {
    agent any

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

        stage('Test') {

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
                    ls -R test-results || true
                '''

            }
        }
    post {
        always {
            junit 'test-results/junit.xml'
            //archiveArtifacts artifacts: 'test-results/junit.xml', fingerprint: true
        }
    }   

    }
}