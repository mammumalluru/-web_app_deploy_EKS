pipeline {

    agent any

    environment {
        ECR_REPO_NAME = "web-app-deploy"
        IMAGE_TAG = "v1-${BUILD_NUMBER}"
        AWS_REGION = "us-east-1"
        AWS_ACCOUNT_ID = "047719616549"
    }

    stages {

        stage('Git Checkout') {
            steps {
                echo "Pulling latest code..."
                git branch: 'main',
                    url: 'https://github.com/mammumalluru/-web_app_deploy_EKS.git',
                    credentialsId: 'git-creds'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image..."
                    sh """
                        docker build -t ${ECR_REPO_NAME}:${IMAGE_TAG} .
                    """
                }
            }
        }

        stage('Login to ECR') {
            steps {
                script {
                    echo "Logging in to Amazon ECR..."
                    sh """
                        aws ecr get-login-password --region ${AWS_REGION} \
                        | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                    """
                }
            }
        }

        stage('Tag & Push Image to ECR') {
            steps {
                script {
                    echo "Tagging and pushing Docker image to ECR..."
                    sh """
                        docker tag ${ECR_REPO_NAME}:${IMAGE_TAG} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}
                        docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }
    }
}
