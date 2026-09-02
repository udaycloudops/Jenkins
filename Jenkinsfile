pipeline {
    agent any

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

    post {
        always {
            echo 'Pipeline execution completed.'
        }
    }
}
