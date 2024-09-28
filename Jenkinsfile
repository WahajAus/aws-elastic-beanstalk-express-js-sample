pipeline {
  agent {
    docker {
      image 'node:16'
      args '-v /var/run/docker.sock:/var/run/docker.sock'
    }
  }
  stages {
    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }
    stage('Build') {
      steps {
        sh 'npm run build'
      }
    }
    stage('Test') {
      steps {
        sh 'npm test'
      }
    }
    stage('Security Scan') {
      steps {
        sh 'npm install -g snyk'
        sh 'snyk test'
  }
}
  }
}
