pipeline {
    agent any

    environment {
        IMAGE_NAME = "varshith57/movie-recommendation-system"
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/saivarshithredd/Movie-Recommendation-System.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    bat 'docker context use default'
                    withDockerRegistry([credentialsId: 'docker_hub_credentials', url: '']) {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    bat 'docker stop movie-recommendation || exit 0'
                    bat 'docker rm movie-recommendation || exit 0'
                    bat "docker run -d -p 5000:5000 --name movie-recommendation ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
