pipeline {
    agent any

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
    }
}