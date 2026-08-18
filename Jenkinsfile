pipeline {

    agent any

    environment {
        IMAGE_NAME = "nikhil031020/day30-devops-app"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code'
            }
        }

        stage('Application Test') {
            steps {
                echo 'Testing application'

                bat 'findstr /C:"Welcome to My DevOps Project" app/index.html'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image'

                bat 'docker build -t %IMAGE_NAME%:latest .'
            }
        }

        stage('Docker Compose Test') {
            steps {

                echo 'Starting Docker Compose'

                bat 'docker compose up -d --build'

                bat 'docker compose ps'
            }
        }

        stage('Web Application Test') {
            steps {

                echo 'Testing web container'

                bat 'curl -f http://localhost:8083'
            }
        }

        stage('Docker Image Check') {
            steps {

                bat 'docker images %IMAGE_NAME%'
            }
        }

        stage('Docker Push') {
            steps {

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {

                    bat 'docker login -u %DOCKER_USERNAME% -p %DOCKER_PASSWORD%'

                    bat 'docker push %IMAGE_NAME%:latest'
                }
            }
        }

    }

    post {

        always {

            echo 'Cleaning up Docker Compose'

            bat 'docker compose down'
        }

        success {

            echo '===================================='
            echo 'DAY 30 PIPELINE SUCCESSFUL'
            echo '===================================='
        }

        failure {

            echo '===================================='
            echo 'DAY 30 PIPELINE FAILED'
            echo '===================================='
        }
    }
}
