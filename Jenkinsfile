pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-app"
        DOCKER_HUB_REPO = "vijayalakshmee/my-app" // ✅ Corrected username
        DOCKER_REGISTRY_CREDENTIALS = "docker"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/VIJAYALAKSHMEE/task2jenkins.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_HUB_REPO}:${BUILD_NUMBER} ."
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                script {
                    withDockerRegistry(credentialsId: docker, url: '') {
                        sh "docker push ${DOCKER_HUB_REPO}:${BUILD_NUMBER}"
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh "docker stop ${IMAGE_NAME} || true && docker rm ${IMAGE_NAME} || true"
                    sh "docker run -d -p 3000:3000 --name ${IMAGE_NAME} ${DOCKER_HUB_REPO}:${BUILD_NUMBER}"
                }
            }
        }
    }

    post {
        success {
            echo "Deployment successful! 🎉"
        }
        failure {
            echo "Deployment failed! ❌"
        }
    }
}
