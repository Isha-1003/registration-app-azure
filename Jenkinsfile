pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'ishaavi/registration-app'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Isha-1003/registration-app-azure.git'
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
        withCredentials([usernamePassword(credentialsId: 'docker-hub-password', usernameVariable: 'DOCKER_HUB_USER', passwordVariable: 'DOCKER_HUB_PASS')]) {
            sh 'echo $DOCKER_HUB_PASS | docker login -u $DOCKER_HUB_USER --password-stdin'
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
