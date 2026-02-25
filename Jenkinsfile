pipeline {
    agent any

    stages {
      
        stage('building image && testing') {
            steps {
                echo 'building image...'
                sh 'docker-compose up --build -d'
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