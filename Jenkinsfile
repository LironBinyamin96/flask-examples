pipeline {
    agent any

    environment {
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Docker Hub Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS' )]) {
                    script {
                        env.IMAGE_NAME = "${DOCKER_USER}/flask-example"
                    }
                    
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
            }
        }


         stage('Snyk Security Scan') {
            steps {
                snykSecurity snykInstallation: 'snyk-image-test', snykTokenId: 'snyk-token'
            }
        }

        stage('Logout from DockerHub') {
            steps {
                sh 'docker logout'
            }
        }
    }
}
