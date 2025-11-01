pipeline {
    agent any

    environment {
        IMAGE_NAME = "hello-devops"
        CONTAINER_NAME = "hello-devops-container"
        HOST_PORT = "5050"
        CONTAINER_PORT = "5000"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Reusing repository checkout (checkout scm)'
                // Reuses the SCM checkout Jenkins used to load this Jenkinsfile
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "🚀 Building Docker image..."
                    sh "docker build -t ${env.IMAGE_NAME} ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    echo "🧩 Running Docker container..."
                    sh "docker rm -f ${env.CONTAINER_NAME} || true"
                    sh "docker run -d -p ${env.HOST_PORT}:${env.CONTAINER_PORT} --name ${env.CONTAINER_NAME} ${env.IMAGE_NAME}"
                }
            }
        }

        stage('Verify Container') {
            steps {
                script {
                    echo "🔍 Checking running containers..."
                    sh 'docker ps'
                    echo "✅ Container running (if everything succeeded) at http://localhost:${HOST_PORT}"
                }
            }
        }
    }

    post {
        success { echo '✅ Docker image built and container started successfully!' }
        failure { echo '❌ Build failed. Check logs for errors.' }
    }
}
