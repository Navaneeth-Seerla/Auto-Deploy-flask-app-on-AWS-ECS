pipeline {
    agent any

    environment {
        AWS_REGION = "us-east-1"
        ECR_REPO = "577638375557.dkr.ecr.us-east-1.amazonaws.com/flask-app"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Navaneeth-Seerla/Auto-Deploy-flask-app-on-AWS-ECS.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${ECR_REPO}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Login to ECR') {
            steps {
                script {
                    sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REPO}"
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    sh "docker push ${ECR_REPO}:${IMAGE_TAG}"
                }
            }
        }

        stage('Deploy to ECS') {
            steps {
                script {
                    sh """
                        aws ecs update-service \
                        --cluster flask-cluster \
                        --service flask-service \
                        --force-new-deployment \
                        --region ${AWS_REGION}
                    """
                }
            }
        }
    }
}
