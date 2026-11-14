
pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'ap-southeast-2'
        ECR_REPOSITORY = '455012390845.dkr.ecr.ap-southeast-2.amazonaws.com/digital-canteen'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t digital-canteen:1.0 .'
            }
        }

        stage('Verify Docker Image') {
            steps {
                bat 'docker images digital-canteen'
            }
        }

        stage('Login to AWS ECR') {
            steps {
                withCredentials([
                    string(credentialsId: 'aws-access-key', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws-secret-key', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    bat '''
                    "C:\\Program Files\\Amazon\\AWSCLIV2\\aws.exe" sts get-caller-identity
                    "C:\\Program Files\\Amazon\\AWSCLIV2\\aws.exe" ecr get-login-password --region %AWS_DEFAULT_REGION% | docker login --username AWS --password-stdin %ECR_REPOSITORY%
                    '''
                }
            }
        }

        stage('Push to AWS ECR') {
            steps {
                bat '''
                docker tag digital-canteen:1.0 %ECR_REPOSITORY%:latest
                docker push %ECR_REPOSITORY%:latest
                '''
            }
        }
    }
}
