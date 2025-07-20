pipeline {
    agent any

    environment {
        IMAGE_NAME = "python-docker:${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo '🔄 Checking out code...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "🐳 Building Docker image: ${IMAGE_NAME}"
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "📦 Simulating image push to ECR: ${IMAGE_NAME}"
                echo "This would be: docker push <account>.dkr.ecr.<region>.amazonaws.com/$IMAGE_NAME"
            }
        }

        stage('Deploy to QA') {
            steps {
                echo "🚀 Simulating deploy to EKS QA cluster"
                echo "This would be: helm upgrade --install myapp ./helm-chart"
            }
        }

        stage('Post Actions') {
            steps {
                echo "✅ Build complete. Notifying team..."
                echo "🧹 Cleaning up build resources..."
            }
        }
    }
}
