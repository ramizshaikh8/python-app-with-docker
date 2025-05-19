pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }

        stage('Git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ramizshaikh8/python-app-with-docker.git'
            }
        }

        stage('Docker Build Image') {
            steps {
                sh 'docker build -t my-python-app .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 5053:5000 my-python-app'
            }
        }
    }
}
