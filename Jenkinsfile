pipeline {
    agent any

    environment {
        IMAGE_NAME = 'my-python-app'
        CONTAINER_NAME = 'my-python-app-container'
        HOST_PORT = '5051'
        CONTAINER_PORT = '5000'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ramizshaikh8/python-app-with-docker.git'
            }
        }

        stage('Test') {
            steps {
                git 'https://github.com/ramizshaikh8/python-app-with-docker.git'
                echo 'No tests defined — skipping for now.'
            }
        }

        stage('Build Docker Image') {
            steps {
                git 'https://github.com/ramizshaikh8/python-app-with-docker.git'
                sh '''
                    docker build -t $IMAGE_NAME .
                    docker image ls | grep $IMAGE_NAME
                '''
            }
        }

        stage('Deploy Docker Container') {
            steps {
                sh '''
                    echo "[INFO] Stopping and removing existing container..."
                    docker rm -f $CONTAINER_NAME || true
                    sleep 2
                    echo "[INFO] Running new container..."
                    docker run -d --name $CONTAINER_NAME -p $HOST_PORT:$CONTAINER_PORT $IMAGE_NAME
                '''
            }
        }

        stage('Run') {
            steps {
                sh '''
                    echo "[INFO] Checking if container is running..."
                    docker ps | grep $CONTAINER_NAME || echo "Container failed to start"

                    echo "[INFO] Attempting curl check (if applicable)..."
                    curl -s http://localhost:$HOST_PORT || echo "No response from app"
                '''
            }
        }
    }

    post {
        success {
            echo '✅ All stages completed.'
        }
        failure {
            echo '❌ Pipeline failed.'
        }
    }
}
