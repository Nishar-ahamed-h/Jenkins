pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')  // Docker Hub credentials
        IMAGE_NAME = "nishar8921/nginx"                         // Image name for Trivy scan
    }

    stages {
        /*
        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'docker login -u $USERNAME -p $PASSWORD'
                }
            }
        }

        stage('Trivy Scan') {
            steps {
                script {
                  sh "trivy image $IMAGE_NAME"
                }
            }   
        }
        */

        stage('Checkout') {
            steps {
                // Checkout code from version control
                git branch:'main', url: 'https://github.com/Nishar-ahamed-h/Test_1.git'
            }
        }
 
        stage('Gitleaks Scan') {
            steps {
                // Run Gitleaks to detect leaks
                sh 'gitleaks detect --source . -v || true'
            }
        }
    }

    post {
        success {
            // Example: Notify on success
            echo 'Pipeline succeeded! Gitleaks did not find any leaks.'
        }
        failure {
            // Example: Notify on failure
            echo 'Pipeline failed! Gitleaks detected potential leaks.'
        }
    }
}
