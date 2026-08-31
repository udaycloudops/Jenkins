pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code from repository...'
            }
        }

        stage('Build') {
            steps {
                echo 'Building application...'
            }
        }

        stage('Test') {
            steps {
                echo 'Running automated tests...'
            }
        }

        stage('Quality Gate') {
            steps {
                echo 'Checking code quality and static analysis...'
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging artifacts...'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to environment...'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
    }
}
