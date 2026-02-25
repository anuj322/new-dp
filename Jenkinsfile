pipeline {
    agent any

    environment {
        EC2_HOST = "ec2-13-233-123-72.ap-south-1.compute.amazonaws.com"
        PROJECT_DIR = "/home/ubuntu/flask-app"
        GIT_REPO = "https://github.com/anuj322/new-dp.git"
        GIT_BRANCH = "dev"
    }

    stages {

        stage('Deploy to EC2') {
            steps {

                sshagent(['my-laptop']) {

                    sh """
                    ssh -o StrictHostKeyChecking=no ubuntu@$EC2_HOST '
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