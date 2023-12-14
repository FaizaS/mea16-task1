pipeline {
    agent any
    stages {
        stage('Build') {
            steps {

                sh 'docker build -t faizashahid/task1kube:latest -t faizashahid/task1kube:v${BUILD_NUMBER} .'
            }
        }

        stage('Push') {
            steps {
                sh '''
                docker push faizashahid/task1kube:latest
                docker push faizashahid/task1kube:v${BUILD_NUMBER}
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f ./kubernetes
                kubectl set image deployment/flask-deployment task1=faizashahid/task1kube:v${BUILD_NUMBER}
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