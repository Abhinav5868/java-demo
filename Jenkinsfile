pipeline {
    agent any
    
    environment {
        DOCKER_CREDENTIALS_ID = 'dckr_pat_6wE-n-mRC0QWm3-v60ityqKX-9Y'  // Docker Hub credentials ID
        DOCKER_USERNAME = 'abhinav5868'  // Docker Hub username
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Use the Git commit hash as the image tag
                    def imageTag = "${GIT_COMMIT}"
                    echo "Building Docker Image: ${DOCKER_USERNAME}/demo-app:${imageTag}"
                    docker.build("${DOCKER_USERNAME}/demo-app:${imageTag}")
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    def imageTag = "${GIT_COMMIT}"
                    echo "Pushing Docker Image: ${DOCKER_USERNAME}/demo-app:${imageTag}"
                    
                    // Push to Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_CREDENTIALS_ID) {
                        docker.image("${DOCKER_USERNAME}/demo-app:${imageTag}").push()
                    }
                }
            }
        }
    }
}
