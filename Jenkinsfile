pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        ECR_REPO = '577638375557.dkr.ecr.us-east-1.amazonaws.com/flask-app'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Navaneeth-Seerla/Auto-Deploy-flask-app-on-AWS-ECS.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t flask-app .'
            }
        }

        stage('Login to ECR') {
            steps {
                bat 'aws ecr get-login-password --region %AWS_REGION% | docker login --username AWS --password-stdin %ECR_REPO%'
            }
        }

        stage('Push to ECR') {
            steps {
                bat 'docker tag flask-app:latest %ECR_REPO%:latest'
                bat 'docker push %ECR_REPO%:latest'
            }
        }

        stage('Deploy to ECS') {
            steps {
                bat '''
                aws ecs update-service ^
                  --cluster flask-cluster ^
                  --service flask-service ^
                  --force-new-deployment ^
                  --region %AWS_REGION%
                '''
            }
        }
    }
}
