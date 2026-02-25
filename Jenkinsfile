pipeline {
    agent any

    environment {
        EC2_HOST = "ec2-65-0-81-201.ap-south-1.compute.amazonaws.com"
        PROJECT_DIR = "/home/ubuntu/flask-app"
        GIT_REPO = "https://github.com/anuj322/new-dp.git"
        GIT_BRANCH = "main"
        AWS_REGION = "ap-south-1"
    }

    stages {

        stage('Deploy to EC2') {
            steps {

                withCredentials([
                    sshUserPrivateKey(credentialsId: 'ec2-ssh-key',
                                      keyFileVariable: 'SSH_KEY',
                                      usernameVariable: 'SSH_USER'),
                    aws(credentialsId: 'aws admin',
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')
                ]) {

                    sh """
                    ssh -i $SSH_KEY -o StrictHostKeyChecking=no $SSH_USER@$EC2_HOST '
                        export AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID
                        export AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY
                        export AWS_DEFAULT_REGION=$AWS_REGION

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