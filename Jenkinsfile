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
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful'
        }

        failure {
            echo 'Deployment Failed'
        }
    }
}

