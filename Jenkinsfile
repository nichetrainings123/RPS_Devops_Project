pipeline {
    agent any

    environment {
        IMAGE_NAME = "nichetrainings123/rps-devops-project"
        IMAGE_TAG  = "latest"
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
                  echo "Workspace content:"
                  ls -la
                  docker build -t $IMAGE_NAME:$IMAGE_TAG .
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
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
            }
        }
    }

    post {
        success {
            echo "Docker image pushed successfully 🚀"
        }
        failure {
            echo "Pipeline failed ❌"
        }
    }
}
