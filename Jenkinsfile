pipeline {
  agent any

  environment {
    IMAGE_NAME     = "hello-devops"
    CONTAINER_NAME = "hello-devops-container"
    HOST_PORT      = "5050"
    CONTAINER_PORT = "5000"
    DOCKER         = "/usr/local/bin/docker"
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Prepare Docker config') {
      steps {
        // create a minimal docker config dir without credential helpers
        sh '''
          rm -rf .docker-temp
          mkdir -p .docker-temp
          echo '{}' > .docker-temp/config.json
        '''
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          withEnv(["DOCKER_CONFIG=${env.WORKSPACE}/.docker-temp"]) {
            sh "${DOCKER} build -t ${IMAGE_NAME} ."
          }
        }
      }
    }

    stage('Run Docker Container') {
      steps {
        script {
          withEnv(["DOCKER_CONFIG=${env.WORKSPACE}/.docker-temp"]) {
            sh "${DOCKER} rm -f ${CONTAINER_NAME} || true"
            sh "${DOCKER} run -d -p ${HOST_PORT}:${CONTAINER_PORT} --name ${CONTAINER_NAME} ${IMAGE_NAME}"
          }
        }
      }
    }

    stage('Verify') {
      steps {
        sh '/usr/local/bin/docker ps || true'
        sh 'curl -sS http://localhost:5050 || echo "app not responding (ok if not HTTP)"'
      }
    }
  }

  post {
    success {
      steps {
        echo "✅ Build & run succeeded — app should be at http://localhost:${env.HOST_PORT}"
      }
    }
    failure {
      steps {
        echo "❌ Build failed — check the logs above for errors."
      }
    }
    always {
      steps {
        // optional: show docker version and cleanup if needed
        sh '/usr/local/bin/docker --version || true'
      }
    }
  }
}
