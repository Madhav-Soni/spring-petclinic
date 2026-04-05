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

        stage('Security Scan') {
            steps {
                sh 'trivy image --timeout 10m petclinic-app || true'
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