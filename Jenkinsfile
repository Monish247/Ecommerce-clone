pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'your_dockerhub_username/your_repository'
        DOCKER_CREDENTIALS_ID = 'your_docker_credentials_id'
    }

    stages {
        stage('Checkout') {
            steps {
                echo "git checkout started"
               // git 'https://github.com/Monish247/Ecommerce-clone.git'
            }
        }
        stage('Build') {
            steps {
                echo "Build stage started"
                sh '''
                python -m venv venv
                . venv/bin/activate
                pip install -r requirements.txt
                '''
            }
        }
        stage('Test') {
            steps {
                echo "Test stage started"
                sh '''
                . venv/bin/activate
                pytest
                '''
            }
        }
        stage('Build Docker Image') {
            steps {
                echo "Building Docker image"
                script {
                    docker.build("${env.DOCKER_HUB_REPO}:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                echo "Pushing Docker image to Docker Hub"
                script {
                    docker.withRegistry('https://index.docker.io/v1/', env.DOCKER_CREDENTIALS_ID) {
                        docker.image("${env.DOCKER_HUB_REPO}:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying application"
                sh 'echo "Deploying application..."'
            }
        }
    }
    
    post {
        always {
            echo "Cleaning up workspace"
            cleanWs()
        }
        success {
            echo "Build, Tests, and Docker Image Push succeeded!"
        }
        failure {
            echo "Build, Tests, or Docker Image Push failed!"
        }
    }
}
