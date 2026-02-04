pipeline {
    agent any

    environment {
        IMAGE_NAME = "nichetrainings123/rps-devops-project"
        IMAGE_TAG  = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/nichetrainings123/RPS_Devops_Project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                  set -e
                  echo "Workspace content:"
                  ls -la
                  docker build -t $IMAGE_NAME:$IMAGE_TAG .
                  docker tag $IMAGE_NAME:$IMAGE_TAG $IMAGE_NAME:latest
                '''
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                sh '''
                  docker push $IMAGE_NAME:$IMAGE_TAG
                  docker push $IMAGE_NAME:latest
                '''
            }
        }
    }

    post {
        success {
            echo "Docker images pushed successfully 🚀"
        }
        failure {
            echo "Pipeline failed ❌"
        }
    }
}
