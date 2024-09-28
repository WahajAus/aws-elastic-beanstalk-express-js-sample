pipeline {
    agent {
        docker {
            image 'node:16'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    stages {
        stage('Dependencies Installation') {
            steps {
                sh 'npm install --save'
                echo 'Installing Finished'

                sh 'npm install snyk --save-dev'
                echo 'Snyk Installation completed'

                withCredentials([string(credentialsId: 'snyk_token', variable: 'SNYK_TOKEN')]) {
                    sh './node_modules/.bin/snyk auth $SNYK_TOKEN'
                    echo 'Snyk Authentication Completed'
                }
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
                echo 'Test Phase Completed'
            }
            post {
                success {
                    echo 'Tests passed!'
                }
                failure {
                    echo 'Some tests failed. Check logs for details.'
                }
            }
        }
        stage('Snyk Security Scan Phase') {
            steps {
                sh 'snyk test'
                echo 'Security Scan Completed'
            }
            post {
                success {
                    echo 'Security Scan passed!'
                }
                failure {
                    echo 'Failed due to vulnerabilities.'
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline Completed.'
        }
    }
}
