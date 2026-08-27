pipeline {
    agent any

    tools {
        jdk 'JDK17'
        maven 'Maven3.9'
    }

    environment {
        APP_NAME         = 'demo-web-app'
        DOCKER_HUB_USER  = 'udaycloudops' // Change to your Docker Hub username
        IMAGE_NAME       = "${DOCKER_HUB_USER}/${APP_NAME}"
        IMAGE_TAG        = "${BUILD_NUMBER}"
        SONAR_SERVER     = 'SonarQubeServer' // Defined in Jenkins System Settings
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/udaycloudops/Jenkins.git'
            }
        }

        stage('Build & Unit Test') {
            steps {
                echo 'Building Java Web Application...'
                sh '/usr/share/maven clean package -DskipTests=false'
            }
        }

        stage('SonarQube Code Analysis') {
            steps {
                withSonarQubeEnv("${SONAR_SERVER}") {
                    sh '/usr/share/maven sonar:sonar'
                }
            }
        }

        stage('Quality Gate Check') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    // Requires SonarQube Scanner plugin configured with Webhook
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Dockerize Image') {
            steps {
                script {
                    echo "Building Docker image: ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} -t ${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Push to Container Registry') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                        sh "docker push ${IMAGE_NAME}:latest"
                    }
                }
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    echo "Deploying ${IMAGE_NAME}:${IMAGE_TAG} container locally..."
                    // Stop & remove existing container if running, then start new one
                    sh '''
                        docker stop demo-web-app || true
                        docker rm demo-web-app || true
                        docker run -d --name demo-web-app -p 8080:8080 ${IMAGE_NAME}:${IMAGE_TAG}
                    '''
                }
            }
        }
    }

    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
        success {
            echo "Pipeline completed successfully! Application deployed to http://localhost:8080"
        }
        failure {
            echo "Pipeline failed! Please check console logs."
        }
    }
}
