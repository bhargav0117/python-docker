pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        IMAGE_NAME = "python-docker"
        ECR_ACCOUNT_ID = "975050343222"
        ECR_REPO = "${ECR_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_NAME}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
                }
            }
        }

        stage('Login to ECR') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-ecr-creds', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                        aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
                        aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
                        aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin $ECR_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com
                    '''
                }
            }
        }

        stage('Tag and Push to ECR') {
            steps {
                sh '''
                    docker tag $IMAGE_NAME:$BUILD_NUMBER $ECR_REPO:$BUILD_NUMBER
                    docker push $ECR_REPO:$BUILD_NUMBER
                '''
            }
        }
    }
}
