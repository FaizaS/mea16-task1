pipeline {
    agent any
    stages {
        stage('Build') {
            steps {

                sh 'docker build -t faizashahid/flask-jenk:latest -t faizashahid/flask-jenk:v${BUILD_NUMBER} .'
            }
        }

        stage('Push') {
            steps {
                sh '''
                docker push faizashahid/flask-jenk:latest
                docker push faizashahid/flask-jenk:v${BUILD_NUMBER}
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f ./kubernetes
                kubectl rollout restart deployment flask-deployment
                kubectl rollout restart deployment nginx-deployment
                '''
            }
        }

        stage('CleanUp') {
            steps {
                sh '''
                docker system prune -f 
                '''
            }
        }
    }
}