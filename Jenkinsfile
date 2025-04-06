pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t lironbinyamin/flask-container-snyk:latest .'
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                script {
                    sh 'docker push lironbinyamin/flask-container-snyk:latest'
                }
            }
        }
    }
}
