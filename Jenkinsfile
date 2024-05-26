pipeline {
    agent any

    environment {
        // Use the credentials ID from Jenkins
        GIT_CREDENTIALS_ID = 'new-id'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout code from version control using stored credentials
                    withCredentials([usernamePassword(credentialsId: GIT_CREDENTIALS_ID, usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        sh """
                            git config --global credential.helper store
                            git clone https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/your-repo/your-project.git
                        """
                    }
                }
            }
        }

        stage('Display HTML') {
            steps {
                script {
                    // Assume the HTML file is in the root of the repository
                    publishHTML(target: [
                        reportName: 'Webpage',
                        reportDir: 'your-project',
                        reportFiles: 'webpage.html',
                        keepAll: true,
                        alwaysLinkToLastBuild: true,
                        allowMissing: false
                    ])
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace
            cleanWs()
        }
        success {
            // Notify success
            mail to: 'team@example.com',
                 subject: "SUCCESS: Build ${env.BUILD_NUMBER}",
                 body: "Good news! The build ${env.BUILD_NUMBER} succeeded."
        }
        failure {
            // Notify failure
            mail to: 'team@example.com',
                 subject: "FAILURE: Build ${env.BUILD_NUMBER}",
                 body: "Unfortunately, the build ${env.BUILD_NUMBER} failed. Please check the logs."
        }
    }
}
