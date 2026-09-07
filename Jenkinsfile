pipeline {

    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        AWS_ACCOUNT_ID = '412664885682'
        ECR_REPOSITORY = 'docker-ecr-jenkins'
        IMAGE_NAME = 'docker-ecr-jenkins'
        IMAGE_TAG = 'latest'
        ECR_REGISTRY = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code from GitHub'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image'
                sh 'docker build -t docker-ecr-jenkins:latest .'
            }
        }

        stage('Verify Docker Image') {
            steps {
                echo 'Verifying Docker image'
                sh 'docker images docker-ecr-jenkins'
            }
        }

        stage('AWS Authentication') {
            steps {
                echo 'Checking AWS authentication'
                withCredentials([
                    [$class: 'AmazonWebServicesCredentialsBinding',
                     credentialsId: 'aws-ecr-credentials']
                ]) {
                    sh 'aws sts get-caller-identity'
                }
            }
        }

        stage('Login to ECR') {
            steps {
                echo 'Logging into AWS ECR'
                withCredentials([
                    [$class: 'AmazonWebServicesCredentialsBinding',
                     credentialsId: 'aws-ecr-credentials']
                ]) {
                    sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 412664885682.dkr.ecr.ap-south-1.amazonaws.com'
                }
            }
        }

        stage('Tag Image') {
            steps {
                echo 'Tagging Docker image'
                sh 'docker tag docker-ecr-jenkins:latest 412664885682.dkr.ecr.ap-south-1.amazonaws.com/docker-ecr-jenkins:latest'
            }
        }

        stage('Push Image to ECR') {
            steps {
                echo 'Pushing Docker image to ECR'
                sh 'docker push 412664885682.dkr.ecr.ap-south-1.amazonaws.com/docker-ecr-jenkins:latest'
            }
        }
    }

    post {
        success {
            echo 'Docker image successfully pushed to AWS ECR!'
        }

        failure {
            echo 'Pipeline failed.'
        }
    }
}
