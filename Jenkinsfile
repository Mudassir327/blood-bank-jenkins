pipeline {
    agent any

    tools {
        jdk 'JDK-17'
        maven 'Maven-3.9'
        nodejs 'NodeJS-18'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Mudassir327/blood-bank-jenkins.git'
            }
        }

        stage('Build Backend') {
            steps {
                dir('blood-bank-api') {
                    sh './mvnw clean package -DskipTests'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('blood_bank') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    // Build backend Docker image
                    docker.build('blood-bank-backend', 'blood-bank-api/')

                    // Build frontend Docker image
                    docker.build('blood-bank-frontend', 'blood_bank/')
                }
            }
        }

        stage('Deploy Backend') {
            steps {
                script {
                    // Run backend container
                    docker.run('blood-bank-backend', '-p 8081:8080')
                }
            }
        }

        stage('Deploy Frontend') {
            steps {
                script {
                    // Run frontend container
                    docker.run('blood-bank-frontend', '-p 3000:80')
                }
            }
        }
    }
}
