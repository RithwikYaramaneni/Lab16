pipeline {
    agent any

    environment {
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
                sh 'docker stop test-container || true'
                sh 'docker rm test-container || true'
                // Maps host port 8081 to container port 8080
                sh 'docker run -d --name test-container -p 8081:8080 java-docker-app:${BUILD_NUMBER}'
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