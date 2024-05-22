pipeline {
    agent any
    
    tools {
        jdk 'JDK11' 
    }
    
    stages {
        stage('Build') {
            steps {
                echo 'Application build stage...' 
                sh 'javac program.java'
        }
       }
        stage('Test') {
            steps {
                echo 'Application test stage' 
        }
        }
        stage('Run') {
            steps {
                echo 'Application run stage' 
                sh 'gcloud compute zones list'
                sh 'java program'
            }
        }
    }
}



