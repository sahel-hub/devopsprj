pipeline {
    agent any

    environment {
        IMAGE_NAME = 'bus-booking-site-image'
        CONTAINER_NAME = 'bus-booking-site-container'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                dir('bus-booking-site') {
                    script {
                        bat "docker build -t %IMAGE_NAME% ."
                    }
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    bat "docker stop %CONTAINER_NAME% || echo Not running"
                    bat "docker rm %CONTAINER_NAME% || echo Not found"
                    bat "docker run -d -p 8084:80 --name %CONTAINER_NAME% %IMAGE_NAME%"
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                echo 'Checking if the site is up...'
                bat 'curl http://localhost:8084 || echo Site unreachable'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            bat 'docker stop %CONTAINER_NAME% || echo Already stopped'
            bat 'docker rm %CONTAINER_NAME% || echo Already removed'
        }
    }
}
