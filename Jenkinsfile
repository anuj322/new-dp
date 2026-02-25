pipeline {
    agent any

    stages {
        stage('fetching code') {
            steps {
                echo 'start fetching code...'
                git url: 'https://github.com/anuj322/new-dp.git', branch: 'main'
            }
        }
        stage('building image && testing') {
            steps {
                echo 'building image...'
                sh 'docker compose up --build -d'
            }
        }
    }

    post {
        always {
            echo 'Cleaning workspace...'
            cleanWs()  // Jenkins built-in function to clean the workspace
        }
    }
}