pipeline {
    agent any

    stages {
        stage('Environment Check') {
            steps {
                sh 'docker version'
                sh 'docker compose version'
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker Images...'
                sh 'docker compose -p jenkins-test build'
            }
        }

        stage('Test') {
            steps {
                echo 'Running Django Tests...'
                // Spin up isolation with "-p jenkins-test"
                sh 'docker compose -p jenkins-test run --rm web python manage.py test'
            }
        }

        stage('Deploy (Scale)') {
            steps {
                echo 'Deploying with 3 replicas...'
                // Use default project name for the "live" deployment
                sh 'docker compose up -d --scale web=3 --build'
            }
        }

        stage('Cleanup') {
            steps {
                echo 'Cleaning up test environment...'
                sh 'docker compose -p jenkins-test down'
                sh 'docker image prune -f'
            }
        }
    }
}
