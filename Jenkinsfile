pipeline {
    agent any

    environment {
        DOCKERHUB      = credentials('dockerhub-creds')
        BACKEND_IMAGE  = "TON_USER/projet-backend"
        FRONTEND_IMAGE = "TON_USER/projet-frontend"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build images') {
            steps {
                sh 'docker build -t $BACKEND_IMAGE:${BUILD_NUMBER} -t $BACKEND_IMAGE:latest ./backend'
                sh 'docker build -t $FRONTEND_IMAGE:${BUILD_NUMBER} -t $FRONTEND_IMAGE:latest ./frontend'
            }
        }

        stage('Login Docker Hub') {
            steps {
                sh 'echo $DOCKERHUB_PSW | docker login -u $DOCKERHUB_USR --password-stdin'
            }
        }

        stage('Push images') {
            steps {
                sh 'docker push $BACKEND_IMAGE:${BUILD_NUMBER}'
                sh 'docker push $BACKEND_IMAGE:latest'
                sh 'docker push $FRONTEND_IMAGE:${BUILD_NUMBER}'
                sh 'docker push $FRONTEND_IMAGE:latest'
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
    }
}
