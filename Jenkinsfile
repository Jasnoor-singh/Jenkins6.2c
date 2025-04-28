pipeline {
    agent any

    triggers {
        githubPush()
    }

    environment {
        EMAIL_RECIPIENT   = "singhjasnoor618@gmail.com"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Stage: Checkout - Tool: Git - Retrieving the complete React project codebase from GitHub."
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Stage: Install Dependencies - Tool: npm/yarn - Installing all required npm packages for the React project."
                echo "Command: npm install OR yarn install"
            }
        }

        stage('Build') {
            steps {
                echo "Stage: Build - Tool: npm/yarn - Building React source code into production-ready artifacts."
                echo "Command: npm run build OR yarn build"
            }
        }

        stage('Test') {
            steps {
                echo "Stage: Test - Tool: Jest / Mocha / Enzyme - Running unit and integration tests."
                echo "Command: npm test OR yarn test"
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
                echo "Stage: Code Analysis - Tool: ESLint / SonarQube - Checking for code quality and style issues."
                echo "Command: npx eslint . OR Integrate with SonarQube scanner"
            }
        }

        stage('Security Scan') {
            steps {
                echo "Stage: Security Scan - Tool: npm audit / Snyk - Scanning dependencies for vulnerabilities."
                echo "Command: npm audit OR snyk test"
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
                echo "Stage: Deploy to Staging - Tool: SCP / rsync / Ansible - Deploying build artifacts to the staging server."
                echo "Command Example: scp -r build/ ${env.STAGING_SERVER}:/var/www/html"
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo "Stage: Integration Tests - Tool: Selenium / Cypress - Running end-to-end tests on staging."
                echo "Command: npx cypress run OR selenium test suite execution"
            }
        }

        stage('Deploy to Production') {
            steps {
                echo "Stage: Deploy to Production - Tool: SCP / rsync / Ansible / Jenkins SSH - Deploying to production server."
                echo "Command Example: scp -r build/ ${env.PRODUCTION_SERVER}:/var/www/html"
            }
        }
    }

    post {
        success {
            echo "Pipeline succeeded! ✅"
        }
        failure {
            echo "Pipeline failed! ❌"
        }
    }
}
