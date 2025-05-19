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
                echo 'Running tests...'
                // Optional: Replace with real tests if you have any
                sh 'echo "No tests defined"'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy Docker Container') {
            steps {
                echo "Deploying Docker container..."
                sh '''
                    echo "[INFO] Stopping and removing existing container (if any)..."
                    docker rm -f $CONTAINER_NAME 2>/dev/null || echo "[INFO] No existing container to remove."

                    echo "[INFO] Starting a new container..."
                    docker run -d --name $CONTAINER_NAME -p $HOST_PORT:$CONTAINER_PORT $IMAGE_NAME

                    if [ $? -eq 0 ]; then
                        echo "[SUCCESS] Container started successfully on port $HOST_PORT"
                    else
                        echo "[ERROR] Failed to start the container"
                        exit 1
                    fi
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully.'
        }
        failure {
            echo '❌ Pipeline failed. Check above logs for details.'
        }
    }
}
