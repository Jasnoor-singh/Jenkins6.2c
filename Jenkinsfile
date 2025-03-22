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
                echo "Fetching source code for React project"
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                echo "Installing all required npm packages for React project"
                sh "npm install"
            }
        }
        stage('Build') {
            steps {
                echo "Building React project to generate production-ready artifacts"
                sh "npm run build"
            }
        }
        stage('Test') {
            steps {
                echo "Executing test suite to validate React components and logic"
                sh "npm test"
            }
            post {
                always {
                    emailext(
                        to: "${env.EMAIL_RECIPIENT}",
                        subject: "React Project Test Stage: ${currentBuild.currentResult}",
                        body: "Tests completed with status: ${currentBuild.currentResult}"
                    )
                }
            }
        }
        stage('Code Analysis') {
            steps {
                echo "Analyzing React codebase for style, consistency, and potential issues"
                sh "npm run lint"
            }
        }
        stage('Security Scan') {
            steps {
                echo "Checking for vulnerabilities in React project's dependencies"
                sh "npm audit"
            }
            post {
                always {
                    emailext(
                        to: "${env.EMAIL_RECIPIENT}",
                        subject: "React Project Security Scan: ${currentBuild.currentResult}",
                        body: "Security checks completed with status: ${currentBuild.currentResult}"
                    )
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo "Deploying React project build to staging server"
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo "Validating React application functionality in staging environment"
            }
        }
        stage('Deploy to Production') {
            steps {
                echo "Releasing React application to production server"
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
