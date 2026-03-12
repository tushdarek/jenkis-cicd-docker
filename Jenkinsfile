pipeline {
    agent { label 'docker' }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/tushdarek/jenkis-cicd-docker.git'
            }
        }

        stage('Check Docker') {
            steps {
                sh '''
                if ! command -v docker &> /dev/null
                then
                    echo "Installing Docker..."
                    sudo apt update
                    sudo apt install docker.io -y
                    sudo systemctl start docker
                    sudo systemctl enable docker
                else
                    echo "Docker already installed"
                fi
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t mywebsite .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker stop mywebsite || true
                docker rm mywebsite || true
                docker run -d -p 80:80 --name mywebsite mywebsite
                '''
            }
        }

    }
}