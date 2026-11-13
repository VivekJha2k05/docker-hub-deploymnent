
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Getting project from GitHub'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t devops-website:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Test Docker Image') {
            steps {
                sh '''
                    docker image inspect devops-website:${BUILD_NUMBER}
                '''
            }
        }
    }
}
