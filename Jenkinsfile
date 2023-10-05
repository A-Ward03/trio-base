pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        docker build -t amward03/trio-deploy-flask:latest -t amward03/trio-deploy-flask:v$BUILD_NUMBER .
                        '''
                    } else {
                        sh '''
                        echo "Build Not required"
                        '''
                    }
                }
            }
        }
        stage('Push') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        docker push amward03/trio-deploy-flask:latest
                        docker push amward03/trio-deploy-flask:v$BUILD_NUMBER
                        '''
                    } else {
                        sh '''
                        echo "Push Not required"
                        '''
                    }
                }
            }
        }
        stage('Cleanup') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        docker rmi amward03/trio-deploy-flask:latest
                        docker rmi amward03/trio-deploy-flask:v$BUILD_NUMBER
                        '''
                    } else {
                        sh '''
                        echo "Cleanup Not required"
                        '''
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        kubectl apply -f ./k8s -n pre-prod
                        kubectl rollout restart deployment flask-deployment -n pre-prod
                        '''
                    } else if (env.GIT_BRANCH == 'origin/main') {
                        sh '''
                        kubectl apply -f ./k8s -n prod
                        kubectl rollout restart deployment flask-deployment -n prod
                        '''
                    } else {
                        sh '''
                        echo "Branch not recognised try again"
                        '''
                    }
                }
            }
        }
    }
}