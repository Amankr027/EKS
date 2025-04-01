pipeline {
    agent any

    environment {
        ECR_REGISTRY = '084828599642.dkr.ecr.us-east-1.amazonaws.com'
        IMAGE_NAME = 'eks-repo'
        IMAGE_TAG = "${new Date().format('yyyyMMddHHmmss')}"
        FULL_IMAGE = "${ECR_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/Amankr027/EKS.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $FULL_IMAGE .'
            }
        }

        stage('Push to ECR') {
            steps {
                sh 'aws ecr get-login-password | docker login --username AWS --password-stdin $ECR_REGISTRY'
                sh 'docker push $FULL_IMAGE'
            }
        }

        stage('Deploy to EKS') {
            steps {
                sh 'kubectl set image deployment/nodejs-deployment nodejs-container=$FULL_IMAGE'
            }
        }
    }
}
