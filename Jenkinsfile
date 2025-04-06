pipeline {
    agent any

    environment {
        IMAGE_NAME = "lironbinyamin/flask-container-snyk"
        TAG = "${new Date().format('yyyy-MM-dd')}"
    }

    stages {
        stage('Build') {
            steps {
                echo '🔧 Building Docker image...'
                script {
                    sh 'docker build -t $IMAGE_NAME:$TAG .'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Pushing Docker image to Docker Hub...'
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh '''
                            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                            docker push $IMAGE_NAME:$TAG
                        '''
                    }
                }
            }
        }
    }
}
