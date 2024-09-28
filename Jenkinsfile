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

                sh 'npm install -g snyk' 
                echo 'Snyk Installation completed globally'

                withCredentials([string(credentialsId: 'snyk_token', variable: 'SNYK_TOKEN')]) {
                    sh 'snyk auth $SNYK_TOKEN'
                    echo 'Snyk Authentication Completed'
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
