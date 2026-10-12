pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from Git...'
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
                sh 'docker run -d --name test-container java-docker-app:${BUILD_NUMBER}'
            }
        }
    }
}