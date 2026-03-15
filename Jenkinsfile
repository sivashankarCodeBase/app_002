pipeline {
    agent any

    stages {
        stage('Environment Check') {
            steps {
                // Verify we still have Docker access
                sh 'docker version'
                sh 'docker compose version'
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker Images...'
                // Using "docker compose" (v2) which is what you have installed
                sh 'docker compose build'
            }
        }

        stage('Test') {
            steps {
                echo 'Running Django Tests...'
                // Spin up a temporary container to run tests
                sh 'docker compose run --rm web python manage.py test'
            }
        }

        stage('Deploy (Scale)') {
            steps {
                echo 'Deploying with 3 replicas...'
                // Re-deploy and ensure 3 instances are running
                sh 'docker compose up -d --scale web=3'
            }
        }

        stage('Cleanup') {
            steps {
                echo 'Cleaning up dangling images...'
                sh 'docker image prune -f'
            }
        }
    }
}
