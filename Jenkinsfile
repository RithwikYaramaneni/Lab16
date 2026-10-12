pipeline {
    agent any

    environment {
        // Ensures Maven, Docker, and system binaries are in PATH on macOS
        PATH = "/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from Git repository...'
            }
        }
        stage('Build') {
            steps {
                echo 'Building application with Maven...'
                sh 'mvn clean package'
            }
        }
        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t java-docker-app:${BUILD_NUMBER} .'
            }
        }
        stage('Run Container') {
            steps {
                echo 'Running Docker container...'
                // Stop and remove existing container if running
                sh 'docker stop test-container || true'
                sh 'docker rm test-container || true'
                sh 'docker run -d --name test-container -p 8080:8080 java-docker-app:${BUILD_NUMBER}'
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check build console logs.'
        }
    }
}