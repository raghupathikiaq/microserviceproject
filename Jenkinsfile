pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'git_creds',
                    url: 'https://github.com/raghupathikiaq/microserviceproject.git'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2_creds']) {
                    sh '''
                        ssh -A -o StrictHostKeyChecking=no ubuntu@3.109.109.202 \
                        "set -e &&
                        rm -rf /home/ubuntu/microservices &&
                        git clone --recurse-submodules https://github.com/raghupathikiaq/microserviceproject.git /home/ubuntu/microservices &&
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
