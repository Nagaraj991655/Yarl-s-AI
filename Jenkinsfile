pipeline {
    agent any

    stages {

        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Cleanup Environment') {
            steps {
                sh '''
                docker compose down --remove-orphans || true

                docker container prune -f || true

                docker network prune -f || true

                sudo fuser -k 8000/tcp || true
                sudo fuser -k 4200/tcp || true

                sleep 5
                '''
            }
        }

        stage('Build Images') {
            steps {
                sh '''
                docker compose build --no-cache
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

                sleep 10

                docker compose logs --tail=50
                '''
            }
        }
    }

    post {

        success {
            echo 'Deployment Successful'

            sh '''
            echo "===== RUNNING CONTAINERS ====="
            docker ps

            echo "===== DOCKER COMPOSE STATUS ====="
            docker compose ps
            '''
        }

        failure {
            echo 'Deployment Failed'

            sh '''
            echo "===== DOCKER STATUS ====="
            docker ps -a || true

            echo "===== BACKEND LOGS ====="
            docker compose logs backend || true

            echo "===== FRONTEND LOGS ====="
            docker compose logs frontend || true
            '''
        }
    }
}
