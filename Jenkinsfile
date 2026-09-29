pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Copy Environment File') {
            steps {
                sh '''
                    cp /home/ubuntu/Contact-Management-Web-Application-Template/.env .env
                    chmod 600 .env

                    echo ".env copied successfully"
                    test -r .env
                '''
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker compose build'
            }
        }

        stage('Deploy Application') {
            steps {
                sh 'docker compose up -d'
            }
        }

        stage('Check Containers') {
            steps {
                sh 'docker compose ps'
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                    echo "Waiting for application..."
                    sleep 10

                    echo "Checking backend API..."
                    curl -f http://localhost/api/health

                    echo ""
                    echo "Checking frontend..."
                    curl -f http://localhost/

                    echo ""
                    echo "Application is healthy."
                '''
            }
        }
    }

    post {

        success {
            echo 'Contact Management application deployed successfully.'
        }

        failure {
            echo 'Deployment failed. Check the Jenkins console output.'
        }

        always {
            sh 'docker compose ps || true'
        }
    }
}
