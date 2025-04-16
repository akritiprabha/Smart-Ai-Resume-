pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "resume-analyzer-image"
        DOCKER_CONTAINER = "resume-analyzer-container"
        PORT = "8501"
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Cloning from the main branch
                git branch: 'main', url: 'https://github.com/'
            }
        }

        stage('Remove Old Container and Image') {
            steps {
                echo '=== Stopping and Removing Old Container ==='
                bat "docker stop %DOCKER_CONTAINER% || echo Container not running"
                bat "docker rm %DOCKER_CONTAINER% || echo Container not found"

                echo '=== Removing Old Docker Image ==='
                bat "docker rmi %DOCKER_IMAGE% || echo Image not found or in use"
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '=== Building New Docker Image ==='
                bat "docker build -t %DOCKER_IMAGE% ."
            }
        }

        stage('Run Docker Container') {
            steps {
                echo '=== Running New Container ==='
                bat "docker run -d -p %PORT%:8501 --name %DOCKER_CONTAINER% %DOCKER_IMAGE%"
            }
        }
    }
}
