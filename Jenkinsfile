pipeline {
    agent any

    stages {
        stage('File') {
            steps {
                sh '''
                if [ -d "/var/lib/jenkins/DevOps/" ]; then
                    find "/var/lib/jenkins/DevOps/" -mindepth 1 -delete
                    echo "Contents of /var/lib/jenkins/DevOps/ have been removed."
                else
                    echo "Directory /var/lib/jenkins/DevOps/ does not exist."
                fi
                '''
            }
        }

        stage('Fetch Code') {
            steps {
                sh 'git clone https://github.com/MuhammadTalhaShafique/taskmanagerjenkins.git /var/lib/jenkins/DevOps/php/'
            }
        }

        stage('Build and Start Docker Compose') {
            steps {
                dir('/var/lib/jenkins/DevOps/php/') {
                    sh 'docker compose -p thereactapp down --volumes --remove-orphans'
                    sh 'docker ps -a -q -f name=frontend-react-jenkins | xargs -r docker rm -f'
                    sh 'docker ps -a -q -f name=backend-node-jenkins | xargs -r docker rm -f'
                    sh 'docker ps -a -q -f name=mongo-db-jenkins | xargs -r docker rm -f'
                    
                    sh 'docker compose -p taskmanagerjenkins up -d --build'
                }
            }
        }
    }
}
