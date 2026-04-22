pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'obito991'
        BACKEND_IMAGE = 'obito991/backend'
        FRONTEND_IMAGE = 'obito991/frontend'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Project Structure') {
            steps {
                sh '''
                    set -e
                    test -d frontend
                    test -d backend
                    test -d helm/journey-app
                    echo "Project structure looks good ✅"
                '''
            }
        }



        stage('Build Backend Image') {
            steps {
                sh '''
                    set -e
                    docker build -t ${BACKEND_IMAGE}:${IMAGE_TAG} ./backend
                    docker tag ${BACKEND_IMAGE}:${IMAGE_TAG} ${BACKEND_IMAGE}:latest
                '''
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh '''
                    set -e
                    docker build -t ${FRONTEND_IMAGE}:${IMAGE_TAG} ./frontend
                    docker tag ${FRONTEND_IMAGE}:${IMAGE_TAG} ${FRONTEND_IMAGE}:latest
                '''
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        set -e
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Push Backend Image') {
            steps {
                sh '''
                    set -e
                    docker push ${BACKEND_IMAGE}:${IMAGE_TAG}
                    docker push ${BACKEND_IMAGE}:latest
                '''
            }
        }

        stage('Push Frontend Image') {
            steps {
                sh '''
                    set -e
                    docker push ${FRONTEND_IMAGE}:${IMAGE_TAG}
                    docker push ${FRONTEND_IMAGE}:latest
                '''
            }
        }
    }

    post {
        success {
            echo "Images built and pushed successfully ✅"
        }
        failure {
            echo "Pipeline failed ❌"
        }
        always {
            sh 'docker logout || true'
        }
    }
}