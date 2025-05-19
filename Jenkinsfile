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
                echo 'No tests defined — skipping for now.'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy Docker Container') {
            steps {
                sh '''
                    docker rm -f $CONTAINER_NAME || true
                    sleep 2
                    docker run -d --name $CONTAINER_NAME -p $HOST_PORT:$CONTAINER_PORT $IMAGE_NAME
                '''
            }
        }

        stage('Run') {
            steps {
                sh '''
                    echo "[INFO] Checking if container is running..."
                    docker ps | grep $CONTAINER_NAME || echo "Container failed to start"
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
