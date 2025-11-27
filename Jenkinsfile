pipeline {
    agent any

    stages {

        stage('Pre-check Docker') {
            steps {
                sh 'echo "Checking Docker availability..."'
                sh 'docker --version'
            }
        }

        stage('Checkout') {
            steps {
                echo "Code already checked out by Jenkins"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t bluewave-svc:5 .'
            }
        }

        stage('Stop Existing Container') {
            steps {
                sh '''
                if [ $(docker ps -aq -f name=bluewave-c5) ]; then
                    docker stop bluewave-c5 || true
                    docker rm bluewave-c5 || true
                fi
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 12040:12040 --name bluewave-c5 bluewave-svc:5'
            }
        }
    }

    post {
        success {
            echo "Pipeline Success — Container is running on port 12040!"
        }
        failure {
            echo "Pipeline Failed!"
        }
    }
}

