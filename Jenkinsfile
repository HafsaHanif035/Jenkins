pipeline {
    agent any

    environment {
        // GitHub token ID
        GIT_CREDENTIALS_ID = 'mykey'
    }

    stages {
        stage('Check Credentials') {
            steps {
                script {
                    try {
                        withCredentials([file(credentialsId: 'mykey', variable: 'GCP_KEY_FILE')]) {
                            sh 'gcloud auth activate-service-account --key-file=$GCP_KEY_FILE'
                            echo 'Successfully authenticated with Google service account credentials'
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Failed to authenticate with Google service account credentials: ${e.message}")
                    }
                }
            }
        }

        stage('Checkout') {
            steps {
                script {
                    // Checkout code from GitHub using stored credentials
                    withCredentials([string(credentialsId: GIT_CREDENTIALS_ID, variable: 'GIT_TOKEN')]) {
                        sh """
                            git clone https://${GIT_TOKEN}git@github.com:HafsaHanif035/Jenkins.git
                        """
                    }
                }
            }
        }

        stage('Display HTML') {
            steps {
                script {
                    // Publish the HTML file
                    publishHTML(target: [
                        reportName: 'Webpage',
                        reportDir: 'Jenkins',  // Directory where the repository is cloned
                        reportFiles: 'webpage.html',       // Name of your HTML file
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
            echo 'Successfully built!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
