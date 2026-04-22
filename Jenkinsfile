pipeline {
    agent any

    stages {
        stage('Checkout Info') {
            steps {
                echo "Checking working directory..."
                sh 'pwd'
                sh 'ls -la'
            }
        }

        stage('Verify Project Structure') {
            steps {
                sh 'test -d frontend'
                sh 'test -d backend'
                sh 'test -d helm/journey-app'
                echo "Project structure looks good ✅"
            }
        }
    }
}
