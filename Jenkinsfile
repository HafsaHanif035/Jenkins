pipeline {
    agent any

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
    }

    post {
        success {
            echo 'Successfully built!'
        }
    }
}
