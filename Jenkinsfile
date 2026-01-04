pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'ashish2999/python-app-ec2:${BUILD_NUMBER}'
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
                sh 'docker build -t ${DOCKER_IMAGE} .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withDockerRegistry(
                  credentialsId: 'dockerhub-creds',
                  url: 'https://index.docker.io/v1/'
                ) {
                    sh 'docker push ${DOCKER_IMAGE}'
                }
            }
        }

        stage('Update Deployment File') {
            environment {
                GIT_REPO_NAME = 'Basic-App-Ec2'
                GIT_USER_NAME = 'DAshishReddy'
            }
            steps {
                withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
                    sh '''
                    git config user.email "ashishreddy.dulla@gmail.com"
                    git config user.name "Ashish"
                    BUILD_NUMBER=${BUILD_NUMBER}
                    sed -i "s/replaceImageTag/${BUILD_NUMBER}/g" k8s-manifests/deployment.yml
                    git add k8s-manifests/deployment.yml
                    git commit -m "Update deployment image to version ${BUILD_NUMBER}"
                    git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main
                '''
                }
            }
        }
    }
}
