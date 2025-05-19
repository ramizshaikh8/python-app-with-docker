pipeline {
    agent any

    environment {
        // Adjust if you use virtualenv or specific Python version
        PYTHON_ENV = 'python3'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout your python app repo
                git branch: 'main', url: 'https://github.com/ramizshaikh8/python-app-with-docker.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t my-python-app .'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests inside Docker container...'
                // Run tests by starting a temporary container
                // Assuming your Dockerfile installs test dependencies
                sh 'docker run --rm my-python-app pytest' 
                // Change `pytest` to whatever test command your app uses
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying container...'
                // Stop and remove old container if exists (optional but recommended)
                sh '''
                docker rm -f my-python-app-container || true
                docker run -d --name my-python-app-container -p 5057:5000 my-python-app
                '''
            }
        }
    }

    post {
        always {
            echo 'Cleaning up dangling Docker images...'
            sh 'docker image prune -f'
        }
        failure {
            echo 'Build failed! Notifying team...'
            // Add notification steps here (email/slack etc.)
        }
    }
}
