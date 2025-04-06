pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            agent {
                docker {
                    image 'docker:19.03.12'
                    reuseNode true
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
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
                    image 'docker:19.03.12'
                    reuseNode true
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
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
