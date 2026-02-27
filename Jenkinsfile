pipeline {
    agent any

    stages {

        stage('Checkout from GitHub') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/pramodhpillitla/node-k8s-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '''
                docker build -t node-docker-app:%BUILD_NUMBER% .
                docker tag node-docker-app:%BUILD_NUMBER% pramodhpillitla/node-docker-app:%BUILD_NUMBER%
                '''
            }
        }

        stage('Push Docker Image') {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'docker-hub-cred',
            usernameVariable: 'DOCKER_USER',
            passwordVariable: 'DOCKER_PASS'
        )]) {
            bat '''
            docker login -u %DOCKER_USER% -p %DOCKER_PASS%
            docker push pramodhpillitla/node-docker-app:%BUILD_NUMBER%
            '''
        }
    }
}
        
        stage('Create container') {
            steps {
                bat 'docker run pramodhpillitla/node-docker-app:%BUILD_NUMBER%'
            }
        }



    }
}