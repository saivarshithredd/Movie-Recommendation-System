pipeline {
    agent any

    environment {
        IMAGE_NAME = 'varshith57/movie-recommendation-system'
        IMAGE_TAG = 'latest'
        CONTAINER_NAME = 'movie-recommendation'
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
                    docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                bat 'docker context use default'
                withDockerRegistry([credentialsId: 'docker_hub_credentials', url: '']) {
                    script {
                        docker.image("${IMAGE_NAME}").push("${IMAGE_TAG}")
                    }
                }
            }
        }

        stage('Cleanup Existing Container') {
            steps {
                bat """
                    docker ps -a -q --filter "name=${CONTAINER_NAME}" > container_id.txt
                    for /f %%i in (container_id.txt) do (
                        docker stop %%i
                        docker rm %%i
                    )
                    del container_id.txt
                """
            }
        }

        stage('Run New Container') {
            steps {
                bat "docker run -d -p 5000:5000 --name ${CONTAINER_NAME} ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }
}
