pipeline {
    agent any

    environment {
        // DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')  // Docker Hub credentials
        // IMAGE_NAME = "nishar8921/nginx"                         // Image name for Trivy scan
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

        stage('Checkov Scan') {
            steps {
                script {
                    try {
                        // Run Checkov scan
                        sh 'checkov -d . --soft-fail'

                        // Publish the results
                        sh 'checkov -d . --output-file-path checkov_report.txt'
                        archiveArtifacts 'checkov_report.txt'
                    } catch (Exception e) {
                        // If the Checkov scan fails, print the error message
                        echo "Checkov scan failed: ${e.getMessage()}"
                    }
                }
            }
        }

        /*
        stage('Gitleaks Scan') {
            steps {
                // Run Gitleaks to detect leaks
                sh 'gitleaks detect --source . -v || true'
            }
        }
        */
    }

    post {
        success {
            // Example: Notify on success
            echo 'Pipeline succeeded! Checkov did not find any issues.'
        }
        failure {
            // Example: Notify on failure
            echo 'Pipeline failed! Checkov detected potential issues.'
        }
    }
}
