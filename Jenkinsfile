pipeline {
  agent any
  stages {
    stage('Checkout') { steps { checkout scm } }
    stage('Docker debug') {
      steps {
        sh 'echo "PATH=$PATH"'
        sh 'which docker || true'
        sh 'docker --version || true'
      }
    }
    stage('Build') {
      steps {
        sh 'docker build -t hello-devops .'
      }
    }
  }
}
