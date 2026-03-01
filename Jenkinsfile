pipeline {
    agent any

    environment {
        IMAGE_NAME = "venky-217/static-website"
        CONTAINER_NAME = "static-website-container"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Stop Old Container') {
            steps {
                script {
                    sh """
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                    """
                }
            }
        }

        stage('Run New Container') {
            steps {
                script {
                    sh """
                    docker run -d \
                      --name ${CONTAINER_NAME} \
                      -p 80:80 \
                      ${IMAGE_NAME}:${BUILD_NUMBER}
                    """
                }
            }
        }

        // OPTIONAL: Push to Docker Hub
        /*
        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-credentials-id') {
                        docker.image("${IMAGE_NAME}:${BUILD_NUMBER}").push()
                    }
                }
            }
        }
        */
    }

    post {
        always {
            echo 'Pipeline completed successfully.'
        }
    }
}