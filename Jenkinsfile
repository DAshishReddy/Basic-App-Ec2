pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "ashish2999/python-app"
        DOCKER_TAG = "latest"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'python3 --version'
            }
        }

        stage('Test') {
            steps {
                sh 'echo "No tests added yet"'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withDockerRegistry(
                  credentialsId: 'dockerhub-creds',
                  url: 'https://index.docker.io/v1/'
                ) {
                    sh 'docker push $DOCKER_IMAGE:$DOCKER_TAG'
                }
            }
        }
    }
}
