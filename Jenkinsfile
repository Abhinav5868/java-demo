pipeline {
   agent any
   
   environment {
       DOCKER_CREDENTIALS_ID = 'dckr_pat_6wE-n-mRC0QWm3-v60ityqKX-9Y' // Replace with your Docker ID credentials
       DOCKER_USERNAME = 'abhinav5868'  // Replace with your Docker Hub username
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
                   docker.build("${abhinav5868}/demo-app:${jd1}")
               }
           }
       }
       
       stage('Push Docker Image') {
           steps {
               script {
                   docker.withRegistry('https://registry.hub.docker.com', 'dckr_pat_6wE-n-mRC0QWm3-v60ityqKX-9Y') {
                       docker.image("${abhinav5868}/demo-app:${jd1}").push()
                   }
               }
           }
       }
       
       stage('Deploy') {
           steps {
               sh 'docker-compose up -d'
           }
       }
   }
   
   post {
       always {
           sh 'docker-compose down'
       }
   }
