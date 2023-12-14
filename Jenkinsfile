pipeline {
    agent any
    stages {
        stage('Init') {
            steps {
                script{
                    if (env.GIT_BRANCH == "origin/main") {
                        sh'''
                        Kubectl create namespace prod || echo "Namespace prod already exists"
                        '''
                    } else if (env.GIT_BRANCH == "origin/dev"){
                        sh '''
                        Kubectl create namespace dev || echo "Namespace dev already exists"
                        '''
                    } else {
                        sh '''
                        echo "Branch not recognised"
                        '''
                    }
                }
            }
        }
        stage('Build') {
            steps {
                script{
                    if (env.GIT_BRANCH == "origin/main") {
                        sh 'docker build -t faizashahid/task1kube:latest -t faizashahid/task1kube:prod-v${BUILD_NUMBER} .'
                    } else if (env.GIT_BRANCH == "origin/dev"){
                        sh 'docker build -t faizashahid/task1kube:latest -t faizashahid/task1kube:dev-v${BUILD_NUMBER} .'
                    } else {
                        sh '''
                        echo "Branch not recognised"
                        '''
                    }
                }
            }
        }

        stage('Push') {
            steps {
                script{
                    if (env.GIT_BRANCH == "origin/main") {
                        sh '''
                        docker push faizashahid/task1kube:latest
                        docker push faizashahid/task1kube:prod-v${BUILD_NUMBER}
                        '''
                    } else if (env.GIT_BRANCH == "origin/dev") {
                        sh '''
                        docker push faizashahid/task1kube:latest
                        docker push faizashahid/task1kube:dev-v${BUILD_NUMBER}
                        '''                   
                    } else {
                        sh '''
                        echo "Branch not recognised"
                        '''
                    } 
                }
            }
        }

        stage('Deploy') {
            steps {
                script{
                    if (env.GIT_BRANCH == "origin/main") {
                        sh '''
                        kubectl apply -n prod -f ./kubernetes
                        kubectl set image deployment/flask-deployment task1=faizashahid/task1kube:prod-v${BUILD_NUMBER} -n prod
                        '''
                        } 
                    else if (env.GIT_BRANCH == "origin/dev") {
                        sh '''
                        kubectl apply -n dev -f ./kubernetes
                        kubectl set image deployment/flask-deployment task1=faizashahid/task1kube:dev-v${BUILD_NUMBER} -n dev
                        '''                   
                    } else {
                        sh '''
                        echo "Branch not recognised"
                        '''
                    }
                }
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