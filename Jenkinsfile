pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            agent {
                docker {
                    image 'docker:latest'
                    reuseNode true
                }
            }
            steps {
                checkout scm
                sh 'docker build -t lironbinyamin/flask-container-snyk:latest .'
            }
        }

        stage('Push to DockerHub') {
            agent {
                docker {
                    image 'docker:latest'
                    reuseNode true
                }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKERHUB_USER', passwordVariable: 'DOCKERHUB_PASS')]) {
                    sh '''
                        echo "$DOCKERHUB_PASS" | docker login -u "$DOCKERHUB_USER" --password-stdin
                        docker push lironbinyamin/flask-container-snyk:latest
                    '''
                }
            }
        }
    }
}
