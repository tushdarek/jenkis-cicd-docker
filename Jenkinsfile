pipeline {
    agent { label 'docker' }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/tushdarek/jenkis-cicd-docker.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                echo "Updating packages..."
                sudo apt update -y

                echo "Installing Docker if not present..."
                if ! command -v docker &> /dev/null
                then
                    sudo apt install docker.io -y
                    sudo systemctl start docker
                    sudo systemctl enable docker
                else
                    echo "Docker already installed"
                fi
                '''
            }
        }

        stage('Verify Docker Installation') {
            steps {
                sh '''
                docker --version
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t parallax-website .
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker stop parallax || true
                docker rm parallax || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker run -d -p 80:80 --name parallax parallax-website
                '''
            }
        }

    }
}