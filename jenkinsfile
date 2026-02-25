pipeline {
    agent any

    environment {
        EC2_HOST = "ec2-13-233-123-72.ap-south-1.compute.amazonaws.com"
        EC2_USER = "ubuntu"
        PROJECT_DIR = "/home/ubuntu/flask-app"
        GIT_REPO = "https://github.com/anuj322/new-dp.git"
        GIT_BRANCH = "main"
    }

    stages {

        stage('Deploy on EC2') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ec2-ssh-key', keyFileVariable: 'SSH_KEY', usernameVariable: 'SSH_USER')]) {
                    sh """
                    ssh -i $SSH_KEY -o StrictHostKeyChecking=no $SSH_USER@$EC2_HOST '
                        mkdir -p $PROJECT_DIR
                        cd $PROJECT_DIR
                        if [ ! -d ".git" ]; then
                            git clone -b $GIT_BRANCH $GIT_REPO .
                        else
                            git reset --hard
                            git pull origin $GIT_BRANCH
                        fi
                        docker-compose down || true
                        docker-compose up --build -d
                    '
                    """
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}