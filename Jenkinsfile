pipeline {
  agent any
  stages {
    stage('Checkout') { steps { checkout scm } }

    stage('Build') {
      steps {
        sh '/usr/local/bin/docker build -t hello-devops .'
      }
    }

    stage('Run') {
      steps {
        sh '/usr/local/bin/docker rm -f hello-devops-container || true'
        sh '/usr/local/bin/docker run -d -p 5050:5000 --name hello-devops-container hello-devops'
      }
    }

    stage('Verify') {
      steps {
        sh '/usr/local/bin/docker ps'
        sh '/usr/local/bin/docker --version'
      }
    }
  }
}
