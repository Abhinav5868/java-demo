pipeline {
    agent any

    environment {
        // Docker image details
        DOCKER_IMAGE = "abhinav5868/java-demo"  // Replace with your Docker Hub repo
        DOCKER_TAG = "latest"  // You can use a dynamic tag like commit hash if you prefer
        GIT_REPO = "https://github.com/Abhinav5868/java-demo"  // Your GitHub repo
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from your GitHub repository
                echo 'Cloning repository...'
                git url: "$GIT_REPO"
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    // Build the Docker image using the Dockerfile from your GitHub repo
                    echo 'Building Docker image...'
                    sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'
                }
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    // Log in to Docker Hub using the stored Jenkins credentials
                    echo 'Logging in to Docker Hub...'
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    }
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                script {
                    // Push the built image to Docker Hub
                    echo 'Pushing Docker image to Docker Hub...'
                    sh 'docker push $DOCKER_IMAGE:$DOCKER_TAG'
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    // Clean up the local Docker image after pushing
                    echo 'Cleaning up Docker images...'
                    sh 'docker rmi $DOCKER_IMAGE:$DOCKER_TAG'
                }
            }
        }
    }

    post {
        always {
            // Clean workspace after the pipeline run
            cleanWs()
        }
    }
}
