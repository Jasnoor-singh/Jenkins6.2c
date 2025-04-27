pipeline {
    agent any

    triggers {
        githubPush()
    }

    environment {
        STAGING_SERVER    = "ubuntu@<staging-ec2-ip>"
        PRODUCTION_SERVER = "ubuntu@<production-instance-public-dns>"
        EMAIL_RECIPIENT   = "singhjasnoor1421@gmail.com"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Stage: Checkout - Retrieving the complete React project codebase from GitHub."
                echo "Stage: Checkout - All source files and configuration are now available in the workspace."
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Stage: Install Dependencies - Installing all required npm packages for the React project."
                echo "Stage: Install Dependencies - Ensuring the local environment is configured correctly for the project."
                echo "Simulating dependency installation"
            }
        }

        stage('Build') {
            steps {
                echo "Stage: Build - Converting React source code into production-ready build artifacts."
                echo "Stage: Build - Applying production optimizations to generate a deployable bundle."
                echo "Simulating build process"
            }
        }

        stage('Test') {
            steps {
                echo "Stage: Test - Running the complete test suite to validate component functionality and integration."
                echo "Stage: Test - Ensuring that the application meets quality and functionality requirements."
                echo "Simulating test execution"
            }
            post {
                always {
                    emailext(
                        to: "${env.EMAIL_RECIPIENT}",
                        subject: "React Project Test Stage: ${currentBuild.currentResult}",
                        body: "Test stage completed with status: ${currentBuild.currentResult}"
                    )
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo "Stage: Code Analysis - Examining the code for style, best practices, and potential issues."
                echo "Stage: Code Analysis - Running ESLint and static analysis tools to ensure maintainability."
                echo "Simulating code analysis"
            }
        }

        stage('Security Scan') {
            steps {
                echo "Stage: Security Scan - Scanning the project and its dependencies for known vulnerabilities."
                echo "Stage: Security Scan - Performing an npm audit to detect any security risks in the React project."
                echo "Simulating security scanning"
            }
            post {
                always {
                    emailext(
                        to: "${env.EMAIL_RECIPIENT}",
                        subject: "React Project Security Scan: ${currentBuild.currentResult}",
                        body: "Security scan completed with status: ${currentBuild.currentResult}"
                    )
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo "Stage: Deploy to Staging - Deploying the build artifacts to the staging server."
                echo "Stage: Deploy to Staging - The staging environment is used to simulate production for final validation."
                echo "Simulating deployment to staging"
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo "Stage: Integration Tests on Staging - Running integration tests to verify end-to-end functionality."
                echo "Stage: Integration Tests on Staging - Confirming that all services interact correctly in a staging environment."
                echo "Simulating integration tests on staging"
            }
        }

        stage('Deploy to Production') {
            steps {
                echo "Stage: Deploy to Production - Releasing the finalized React build to the production server."
                echo "Stage: Deploy to Production - The application is now live and accessible to end users."
                echo "Simulating deployment to production"
            }
        }
    }

    post {
        success {
            echo "Pipeline succeeded!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
