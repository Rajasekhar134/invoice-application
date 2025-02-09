pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('docker-hub')
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/praveenmethraskar/invoice-application.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Ensure npm dependencies are installed before building
                    sh 'npm install'
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    // Build the backend image
                    sh 'docker build -t praveenmethraskar/mern-invoice-app:latest -f mern-invoice-app/Dockerfile .'
                    // Build the frontend image
                    sh 'docker build -t praveenmethraskar/client:latest -f client/Dockerfile .'
                }
            }
        }

        stage('Push Docker Images') {
            steps {
                script {
                    // Push images to Docker Hub
                    withDockerRegistry(credentialsId: 'docker-hub') {
                        sh 'docker push praveenmethraskar/mern-invoice-app:latest'
                        sh 'docker push praveenmethraskar/client:latest'
                    }
                }
            }
        }

        stage('Deploy using Docker Compose') {
            steps {
                script {
                    // Shut down any running containers
                    sh 'docker-compose down'
                    // Bring up the services in detached mode
                    sh 'docker-compose up -d'
                }
            }
        }
    }

}
