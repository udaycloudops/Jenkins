pipeline {
    agent any

    environment{
        APP_NAME = "MY-FIRST-APP"
        APP_VERSION = "V1.0"
        TARGET_ENV = "DEV"

        GIT_REPO_URL = 'https://github.com/udaycloudops/Jenkins.git'
        GIT_BRANCH   = 'main'
        GIT_CREDENTIALS_ID = 'eed31d32-38eb-420e-b7d5-1bbe74ed104a'
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out ${env.APP_NAME}..."
                echo "Jenkins Job Name: ${env.JOB_NAME}"
                echo "Jenkins Build Number: ${env.BUILD_NUMBER}"
                echo "Jenkins URL: ${env.JENKINS_URL}"
                echo 'Checking out source code from repository...'
                 git branch: "${env.GIT_BRANCH}",
                     credentialsId: "${env.GIT_CREDENTIALS_ID}",
                     url: "${env.GIT_REPO_URL}"
                    
                     echo 'Source code checked out successfully.'
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
