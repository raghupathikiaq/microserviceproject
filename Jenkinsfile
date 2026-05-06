pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'git-cred',
                    url: 'https://github.com/Siva290395/microservices.git'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-microservices']) {
                    sh '''
                        ssh -A -o StrictHostKeyChecking=no ubuntu@13.218.181.37 \
                        "set -e &&
                        rm -rf /home/ubuntu/microservices &&
                        git clone --recurse-submodules https://github.com/Siva290395/microservices.git /home/ubuntu/microservices &&
                        cd /home/ubuntu/microservices &&
                        chmod +x clone.sh &&
                        ./clone.sh &&
                        docker compose down || true &&
                        docker system prune -f &&
                        docker compose up --build -d"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Success!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
