pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-creds')
        DOCKER_IMAGE = "wienazka/flask-app:latest"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                script {
                    bat 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    bat "echo $DOCKER_HUB_CREDENTIALS_PSW | docker login -u $DOCKER_HUB_CREDENTIALS_USR --password-stdin"
                    bat 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    bat 'helm upgrade --install demo-app ./demo-app --set image.repository=wienazka/flask-app --set image.tag=latest'
                }
            }
        }
    }
}
