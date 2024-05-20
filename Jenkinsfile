// pipeline {
//     agent any
//     stages {
//         stage('Build') {
//             steps {
//                 echo 'Allication build stage...' 
//         }
//        }
//         stage('Test') {
//             steps {
//                 echo 'Allication test stage' 
//         }
//         }
//         stage('Run') {
//             steps {
//                 echo 'Allication run stage' 
//             }
//         }
//     }
// }



pipeline {
    agent any

    tools {
        // Define the JDK and Gradle installations to use
        jdk 'JDK11' // Ensure JDK11 is configured in Jenkins
        gradle 'Gradle6' // Ensure Gradle6 is configured in Jenkins
    }

    environment {
        // Define any necessary environment variables
        JAVA_HOME = tool name: 'JDK11'
        GRADLE_HOME = tool name: 'Gradle6'
        PATH = "${env.GRADLE_HOME}/bin:${env.JAVA_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the current branch
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Run Gradle build
                sh 'gradle clean build'
            }
        }

        stage('Test') {
            steps {
                // Run tests
                sh 'gradle test'
            }
        }

        stage('Package') {
            steps {
                // Package the application
                sh 'gradle assemble'
            }
        }

        stage('Deploy') {
            steps {
                // Deploy the application (for example, to a server or repository)
                // Replace with your actual deployment command
                sh 'gradle deploy'
            }
        }
    }

    post {
        success {
            echo 'Build completed successfully'
        }
        failure {
            echo 'Build failed'
        }
        always {
            // Clean up workspace if needed
            cleanWs()
        }
    }
}

