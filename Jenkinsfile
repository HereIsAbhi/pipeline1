pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // Use the ID you set
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'docker-compose exec -T nodeapp npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'docker-compose exec -T nodeapp npm test'
            }
        }
        stage('Build') {
            steps {
                sh 'docker-compose exec -T nodeapp npm run build'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        def app = docker.build("yourusername/your-repo:${env.BUILD_NUMBER}")
                        app.push()
                    }
                }
            }
        }
    }
} 