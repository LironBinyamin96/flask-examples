pipeline {
    agent any
    stages {
        stage('DockerHub Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKERHUB_USER', passwordVariable: 'DOCKERHUB_PASS')]) {
                    sh '''
                        echo "$DOCKERHUB_PASS" | docker login -u "$DOCKERHUB_USER" --password-stdin
                    '''
                }
            }
        }
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
                    // אם אתה רוצה לבצע את בדיקות ה-Snyk אחרי הדחיפה:
                    sh snykSecurity snykInstallation: 'snyk-image-test', snykTokenId: 'snyk-token'
                }
            }
        }
    }
}

