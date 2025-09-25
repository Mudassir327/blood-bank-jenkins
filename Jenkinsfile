pipeline {
    agent any

    tools {
        jdk 'JDK-17'        // Make sure this JDK is configured in Jenkins
        maven 'Maven-3.9'   // Make sure Maven is configured in Jenkins
        nodejs 'NodeJS-18'  // Make sure NodeJS is configured in Jenkins
    }

    stages {

        stage('Checkout') {
            steps {
                // Checkout code from GitHub
                git branch: 'main', url: 'https://github.com/Mudassir327/blood-bank-jenkins.git'
            }
        }

        stage('Build Backend') {
            steps {
                dir('blood-bank-api') {
                    // Use bat for Windows CMD
                    bat 'mvnw clean package -DskipTests'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('blood_bank/blood_bank') { // Path to your frontend folder
                    bat 'npm install'
                    bat 'npm run build'
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                dir('.') {
                    // Build backend image
                    bat 'docker build -t blood-bank-backend blood-bank-api'
                    // Build frontend image
                    bat 'docker build -t blood-bank-frontend blood_bank/blood_bank'
                }
            }
        }

        stage('Run Docker Containers') {
            steps {
                dir('.') {
                    // Optional: stop/remove previous containers if running
                    bat 'docker rm -f blood-bank-backend || exit 0'
                    bat 'docker rm -f blood-bank-frontend || exit 0'
                    bat 'docker rm -f mysql-db || exit 0'

                    // Run containers
                    bat 'docker-compose up -d'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline finished successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
