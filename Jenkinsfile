pipeline {
    agent any

    stages {

        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Stop Existing Deployment') {
            steps {
                sh '''
                docker compose down --remove-orphans || true

                docker container prune -f || true

                docker network prune -f || true
                '''
            }
        }

        stage('Build Images') {
            steps {
                sh '''
                docker compose build
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                docker compose up -d
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                docker compose ps

                docker ps
                '''
            }
        }
    }

    post {

        success {
            echo 'Deployment Successful'

            sh '''
            docker compose ps
            '''
        }

        failure {
            echo 'Deployment Failed'

            sh '''
            docker compose logs --tail=100 || true
            '''
        }
    }
}
