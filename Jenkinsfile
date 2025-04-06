pipeline {
    agent any

    environment {
        IMAGE_NAME = 'lironbinyamin/flask-container-snyk-pipeline'
        TAG = "${new Date().format('yyyy-MM-dd')}"
    }

    stages {
        stage('Build Docker Image') {
            agent {
                docker {
                    image 'docker:24.0.7'
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
                    reuseNode true
                }
            }
            steps {
                sh "docker build -t ${IMAGE_NAME}:${TAG} ."
            }
        }

        stage('Push to DockerHub') {
            agent {
                docker {
                    image 'docker:24.0.7'
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
                    reuseNode true
                }
            }
            environment {
                DOCKERHUB_USERNAME = credentials('dockerhub-credentials')
            }
            steps {
                sh "echo ${DOCKERHUB_USERNAME_PSW} | docker login -u ${DOCKERHUB_USERNAME_USR} --password-stdin"
                sh "docker push ${IMAGE_NAME}:${TAG}"
            }
        }
    }
}

}
