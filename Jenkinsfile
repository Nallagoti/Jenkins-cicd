pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Building Docker image'
                sh 'docker build -t devops-cicd-task .'
            }
        }
        stage('Test') {
            steps {
                echo 'Checking Docker image'
                sh 'docker image inspect devops-cicd-task'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Starting application'
                sh '''
                    docker rm -f devops-cicd-task-app || true
                    docker run -d \
                      --name devops-cicd-task-app \
                      --restart unless-stopped \
                      -p 8081:80 \
                      devops-cicd-task
                '''
            }
        }
    }
    post {
        success {
            echo 'Docker CI/CD pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
