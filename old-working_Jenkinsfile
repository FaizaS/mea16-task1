pipeline {
    agent any
    stages {

         stage('Init') {

            steps {
                sh '''
                ssh -i ~/.ssh/id_rsa jenkins@10.154.0.23 << EOF    
                docker rm -f $(docker ps -qa) || true
                docker network create new-network || true
                '''
            }

        }

        stage('Build') {

            steps {

                sh 'docker build -t faizashahid/fsapp:latest -t faizashahid/fsapp:v${BUILD_NUMBER} .'
                sh 'docker build -t faizashahid/mynginx:latest -t faizashahid/mynginx:v${BUILD_NUMBER} ./nginx'

            }

        }
        
        stage('Push') {

            steps {

                sh '''
                docker push faizashahid/fsapp:latest
                docker push faizashahid/fsapp:v${BUILD_NUMBER}
                docker push faizashahid/mynginx:latest
                docker push faizashahid/fsapp:v${BUILD_NUMBER}
                '''
            }

        }

        stage('Deploy') {

            steps {
                sh '''
                ssh -i ~/.ssh/id_rsa jenkins@10.154.0.23 << EOF
                docker run -d --name fsapp --network new-network faizashahid/fsapp:latest
                docker run -d -p 80:80 --name mynginx --network new-network faizashahid/mynginx:latest
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