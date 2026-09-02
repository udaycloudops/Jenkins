pipeline {
    agent any

    tools {
        maven 'Maven-3.9.0'     // Automatically configures M2_HOME and adds 'mvn' to PATH
        jdk 'JDK-17'            // Automatically sets JAVA_HOME and updates java binaries
        nodejs 'Node-20'        // Automatically sets node and npm in PATH
    }

    parameters {
        string(name: 'APP_VERSION', defaultValue: 'V1.0', description: 'Version of the application to build')
        choice(name: 'TARGET_ENV', choices: ['DEV', 'QA', 'STAGING', 'PROD'], description: 'Select the target deployment environment')
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Check this to run automated tests')
    }

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
                sh 'echo "Running unit tests..."; exit 0'
                
            }
        }

        stage('Test') {
            when {
                expression { return params.RUN_TESTS == true }
            }
            steps {
                echo 'RUN_TESTS parameter is checked. Running automated tests...'
            }
        }

        // --- GROOVY SCRIPT EXAMPLE STAGE ---
        stage('Quality Gate') {
            steps {
                echo 'Evaluating quality metrics with inline Groovy...'
                
                script {
                    // 1. Groovy Variable Definitions & Calculations
                    def codeCoveragePercentage = 85
                    def minimumThreshold = 80
                    def statusList = ['Linting Passed', 'Security Scan Passed', 'Coverage Check Passed']
                    
                    // 2. Groovy Control Flow (If/Else)
                    if (codeCoveragePercentage >= minimumThreshold) {
                        echo "SUCCESS: Code coverage is ${codeCoveragePercentage}%, which meets the threshold of ${minimumThreshold}%."
                        env.QUALITY_GATE_STATUS = "PASSED"
                    } else {
                        echo "FAILURE: Code coverage is ${codeCoveragePercentage}%, below threshold of ${minimumThreshold}%."
                        env.QUALITY_GATE_STATUS = "FAILED"
                        error("Quality Gate failed due to low coverage!") // Fails the pipeline
                    }

                    // 3. Groovy Loops & Collection Manipulation
                    echo "Summary of Quality Checks:"
                    statusList.each { checkItem ->
                        echo " - Check status: ${checkItem}"
                    }

                    // 4. Using Native Java/Groovy Classes (e.g., Dates)
                    def now = new Date()
                    echo "Quality Gate verified at: ${now.format('yyyy-MM-dd HH:mm:ss')}"
                }
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

    // --- POST ACTIONS SECTION ---
    post {
        // Runs regardless of the build status (Success, Failure, or Aborted)
        always {
            echo "=== [ALWAYS] Pipeline execution finished for Job: ${env.JOB_NAME} #${env.BUILD_NUMBER} ==="
            echo "Cleaning up workspace dynamic files..."
            sh 'rm -rf build_output || true'
        }

        // Runs ONLY if the pipeline completed with a SUCCESS status
        success {
            echo "=== [SUCCESS] Pipeline executed successfully! ==="
            echo "Sending success notification for ${env.APP_NAME} build #${env.BUILD_NUMBER}..."
            // Example action: Trigger downstream job or send Slack/Email success alert
        }

        // Runs ONLY if the pipeline completed with a FAILURE status
        failure {
            echo "=== [FAILURE] Pipeline failed! ==="
            echo "Sending failure alert to team on Slack/Email..."
            echo "Failed during job: ${env.JOB_NAME} - Build URL: ${env.BUILD_URL}"
        }

        // Runs ONLY if the pipeline was manually canceled/stopped by a user
        aborted {
            echo "=== [ABORTED] Pipeline was manually canceled by user or timed out! ==="
            echo "Build #${env.BUILD_NUMBER} was stopped before finishing."
        }
    }
}
