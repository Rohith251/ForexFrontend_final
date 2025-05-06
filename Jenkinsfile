pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'rohith0702/frontend-app'
        CONTAINER_NAME = 'frontend-container'
        PORT = '5173'
    }

    stages {
        stage('Build Frontend Docker Image') {
            steps {
                script {
                    sh "docker build -t $DOCKER_IMAGE:latest ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh "docker push $DOCKER_IMAGE:latest"
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh """
                        docker network ls | grep my-network || docker network create my-network
                        docker ps -a -q --filter 'name=$CONTAINER_NAME' | grep . && docker rm -f $CONTAINER_NAME || echo No container to remove
                        docker run -d -p $PORT:$PORT --name $CONTAINER_NAME --network my-network $DOCKER_IMAGE:latest
                    """
                }
            }
        }
    }

    post {
        success {
            echo ' Frontend app is live on Docker container (localhost:5173)'
        }
        failure {
            echo ' Pipeline failed — check the build logs.'
        }
    }
}
