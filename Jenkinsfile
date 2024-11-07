pipeline {
    agent any
    
    environment {
        DOCKER_CREDENTIALS_ID = 'dckr_pat_6wE-n-mRC0QWm3-v60ityqKX-9Y' // Docker Hub credentials ID
        DOCKER_USERNAME = 'abhinav5868'  // Docker Hub username
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
                script {
                    echo "Working Directory: ${pwd()}"
                }
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
                    // Using Git commit hash for a unique image tag
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
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_CREDENTIALS_ID) {
                        docker.image("${DOCKER_USERNAME}/demo-app:${imageTag}").push()
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    // Check if docker-compose is available
                    sh 'docker-compose -v'
                    sh 'docker-compose up -d' // Start the containers
                }
            }
        }
    }
    
    post {
        always {
            script {
                // Ensure docker-compose down is called to clean up the containers
                sh 'docker-compose down || echo "docker-compose down failed"'
            }
        }
    }
}
