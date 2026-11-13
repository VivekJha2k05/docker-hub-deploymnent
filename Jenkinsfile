
pipeline {
    agent any

    environment {
        DOCKERHUB_REPO = 'vivekkumarjha/devops-website'
        CONTAINER_NAME = 'devops-test'
        HOST_PORT = '8081'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Downloading project from GitHub'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image'

                sh '''
                    docker build \
                        -t "$DOCKERHUB_REPO:$BUILD_NUMBER" \
                        -t "$DOCKERHUB_REPO:latest" .
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo 'Uploading image to Docker Hub'

                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKERHUB_USER',
                    passwordVariable: 'DOCKERHUB_TOKEN'
                )]) {
                    sh '''
                        printf '%s' "$DOCKERHUB_TOKEN" | \
                            docker login \
                            -u "$DOCKERHUB_USER" \
                            --password-stdin

                        docker push "$DOCKERHUB_REPO:$BUILD_NUMBER"
                        docker push "$DOCKERHUB_REPO:latest"

                        docker logout
                    '''
                }
            }
        }

        stage('Deploy Website on EC2') {
            steps {
                echo 'Deploying website container'

                sh '''
                    docker pull "$DOCKERHUB_REPO:$BUILD_NUMBER"

                    docker rm -f "$CONTAINER_NAME" || true

                    docker run -d \
                        --name "$CONTAINER_NAME" \
                        --restart unless-stopped \
                        -p "$HOST_PORT:80" \
                        "$DOCKERHUB_REPO:$BUILD_NUMBER"
                '''
            }
        }

        stage('Verify Website') {
            steps {
                echo 'Checking website availability'

                sh '''
                    sleep 3

                    curl --fail --show-error \
                        http://localhost:$HOST_PORT
                '''
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Website deployed successfully!'
        }

        failure {
            echo 'FAILURE: Check Jenkins Console Output for errors.'
        }

        always {
            echo 'Jenkins pipeline execution completed.'
        }
    }
}
