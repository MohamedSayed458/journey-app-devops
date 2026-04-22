pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'obito991'
        BACKEND_IMAGE = 'obito991/backend'
        FRONTEND_IMAGE = 'obito991/frontend'
        IMAGE_TAG = "${env.BUILD_NUMBER}"

        HOME = '/var/jenkins_home'
        KUBECONFIG = '/var/jenkins_home/.kube/config'
        AWS_SHARED_CREDENTIALS_FILE = '/var/jenkins_home/.aws/credentials'
        AWS_CONFIG_FILE = '/var/jenkins_home/.aws/config'
        AWS_PAGER = ''
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Environment') {
            steps {
                sh '''
                    set -e
                    echo "Current user:"
                    whoami

                    echo "Docker version:"
                    docker --version

                    echo "AWS identity:"
                    aws sts get-caller-identity

                    echo "Kubernetes nodes:"
                    kubectl get nodes

                    echo "Helm version:"
                    helm version

                    echo "Project structure:"
                    test -d frontend
                    test -d backend
                    test -d helm/journey-app
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

        stage('Deploy with Helm') {
            steps {
                sh '''
                    set -e
                    helm upgrade --install journey-app ./helm/journey-app \
                      --set backend.image.repository=${BACKEND_IMAGE} \
                      --set backend.image.tag=${IMAGE_TAG} \
                      --set frontend.image.repository=${FRONTEND_IMAGE} \
                      --set frontend.image.tag=${IMAGE_TAG}
                '''
            }
        }

        stage('Verify Rollout') {
            steps {
                sh '''
                    set -e
                    kubectl rollout status deployment/backend --timeout=180s
                    kubectl rollout status deployment/frontend --timeout=180s
                    kubectl get pods
                    kubectl get svc
                    kubectl get ingress
                '''
            }
        }
    }

    post {
        success {
            echo 'Build, push, and Helm deployment completed successfully ✅'
        }
        failure {
            echo 'Pipeline failed ❌'
        }
        always {
            sh 'docker logout || true'
        }
    }
}