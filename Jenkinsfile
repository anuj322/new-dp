pipeline {
    agent any

    stages {

        stage('Run Container') {
            steps {
                sh 'docker-compose up --build -d'
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