pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Cloning repository...'
                git 'https://github.com/architpatilbtech2022-hash/Task-1.git'  // 🔁 change this to your actual repo URL
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "🚀 Building Docker image..."
                    sh 'docker build -t hello-devops .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    echo "🧩 Running Docker container..."
                    sh 'docker rm -f hello-devops-container || true'
                    sh 'docker run -d -p 5050:5000 --name hello-devops-container hello-devops'
                }
            }
        }

        stage('Verify Container') {
            steps {
                script {
                    echo "🔍 Checking running containers..."
                    sh 'docker ps'
                    echo "✅ Container running successfully at http://localhost:5050"
                }
            }
        }
    }

    post {
        success {
            echo '✅ Docker image built and container started successfully!'
        }
        failure {
            echo '❌ Build failed. Check logs for errors.'
        }
    }
}
