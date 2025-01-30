pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'your-docker-hub-username/registration-app'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/your-github-username/registration-app-sanjaya.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_HUB_REPO:$BUILD_NUMBER .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'docker-hub-password', variable: 'DOCKER_HUB_PASSWORD')]) {
                    sh 'echo $DOCKER_HUB_PASSWORD | docker login -u your-docker-hub-username --password-stdin'
                    sh 'docker push $DOCKER_HUB_REPO:$BUILD_NUMBER'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    kubectl apply -f regapp-deploy.yml
                    kubectl apply -f regapp-service.yml
                '''
            }
        }
    }
}
