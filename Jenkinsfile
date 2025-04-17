pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/saivarshithredd/Movie-Recommendation-System.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('varshith57/movie-recommendation-system')
                }
            }
        }

       stage('Push to Docker Hub') {
    steps {
        // Switch Docker context to one Jenkins can access
        bat 'docker context use default'

        withDockerRegistry([ credentialsId: 'docker_hub_credentials', url: '' ]) {
            script {
                docker.image('varshith57/movie-recommendation-system').push('latest')
            }
        }
    }
}
    }
}
