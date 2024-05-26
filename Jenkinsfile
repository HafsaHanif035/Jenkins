pipeline {
    agent any

    environment {
        // Use the credentials ID from Jenkins
        DEPLOY_CREDENTIALS_ID = 'new-id'
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

        stage('Build') {
            steps {
                // Compile the code
                sh './gradlew build'
            }
        }

        stage('Test') {
            steps {
                // Run the tests
                sh './gradlew test'
            }
            post {
                always {
                    // Archive the test results
                    junit 'build/test-results/**/*.xml'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy the application using stored credentials
                    withCredentials([usernamePassword(credentialsId: DEPLOY_CREDENTIALS_ID, usernameVariable: 'DEPLOY_USERNAME', passwordVariable: 'DEPLOY_PASSWORD')]) {
                        sh """
                            curl -u ${DEPLOY_USERNAME}:${DEPLOY_PASSWORD} -X POST http://deployment-server/api/deploy
                        """
                    }
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
