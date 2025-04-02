pipeline {
    agent any

    environment {
        // Replace 'your-dockerhub-username' with your actual Docker Hub username
        DOCKER_IMAGE = 'your-dockerhub-username/spring-app'
        // This must match the credential ID you created for Docker Hub in Jenkins
        DOCKER_CREDENTIALS = 'docker-hub'
    }

    // Jenkins tools pre-configured in "Global Tool Configuration"
    tools {
        maven 'Maven-3.8.1'
        jdk 'JDK-17'
    }

    stages {
        // Stage 1: Clone code from GitHub
        stage('Checkout Code') {
            steps {
                // Replace 'https://github.com/user/repo.git' with your actual GitHub URL
                git branch: 'main', url: 'https://github.com/user/repo.git'
            }
        }

        // Stage 2: Build the Maven project
        stage('Build Application') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        // Stage 3: Build the Docker image
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        // Stage 4: Push the Docker image to Docker Hub
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS) {
                        docker.image("${DOCKER_IMAGE}:latest").push()
                    }
                }
            }
        }
    }
}
