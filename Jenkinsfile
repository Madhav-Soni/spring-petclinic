pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'chmod +x mvnw'
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t petclinic-app .'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker stop petclinic-container || true'
                sh 'docker rm petclinic-container || true'
                sh 'docker run -d -p 8082:8080 --name petclinic-container petclinic-app'
            }
        }
    }
}