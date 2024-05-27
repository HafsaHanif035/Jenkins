pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'dsadasf' // Replace with your repository URL
            }
        }

        stage('Deploy to Google Cloud') {
            steps {
                script {
                    try {
                        withCredentials([file(credentialsId: 'key', variable: 'GCP_KEY_FILE')]) {
                            sh 'gcloud auth activate-service-account --key-file=$GCP_KEY_FILE'
                            sh 'gcloud config set project engaged-carving-423214-d0' // Replace with your GCP project ID
                            sh 'gcloud compute ssh hafsahanif613@jenkinserver --zone=us-us-west-4b --command="sudo mkdir -p /var/www/html && sudo chmod 777 /var/www/html"' // Create destination directory and set permissions
                            sh 'gcloud compute scp web.html hafsahanif613@jenkinserver:/var/www/html --zone=us-west-4b' // Copy file to destination directory
                            echo 'Successfully deployed web.html to Google Cloud server'
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Failed to deploy web.html to Google Cloud server: ${e.message}")
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Successfully deployed!'
        }
    }
}
