pipeline {
   agent any
   stages {
       stage('Checkout') {
           steps {
               // Clone source code
           }
       }
       stage('Build Docker Image') {
           steps {
               // docker build
           }
       }
       stage('Login to ECR') {
           steps {
               // AWS authentication
           }
       }
       stage('Push to ECR') {
           steps {
               // docker tag + docker push
           }
       }
   }
}
