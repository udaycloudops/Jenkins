pipeline {
    agent any

    tools {
        // Must match the name defined in Jenkins Global Tool Configuration
        jdk 'JDK17'
        maven 'Maven3.9'
    }

    environment {
        APP_NAME = 'java-sample-app'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/example/java-sample-app.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: false
            }
        }
    }

    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
        success {
            echo "Pipeline built successfully!"
        }
        failure {
            echo "Pipeline failed."
        }
    }
}
