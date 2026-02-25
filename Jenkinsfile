pipeline {
    agent any

    stages {
       
        stage('Build Image') {
            steps {
                sh 'docker compose build'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker compose up -d'
            }
        }

        stage('Test') {
            steps {
                sh 'docker ps'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}