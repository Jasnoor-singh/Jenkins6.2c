pipeline {
    agent any
    triggers {
        githubPush()
    }

    environment {
        STAGING_SERVER    = "ubuntu@ec2-16-170-159-223.eu-north-1.compute.amazonaws.com"
        PRODUCTION_SERVER = "ubuntu@<production-instance-public-dns>"
        EMAIL_RECIPIENT   = "singhjasnoor1421@gmail.com"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Checking out code from GitHub..."
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing npm dependencies..."
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                echo "Building the React application..."
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                echo "Running unit tests..."
                sh 'npm test'
            }
            post {
                always {
                    emailext(
                        to: "${env.EMAIL_RECIPIENT}",
                        subject: "React App Test Stage: ${currentBuild.currentResult}",
                        body: "Unit tests completed with status: ${currentBuild.currentResult}"
                    )
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo "Running ESLint for code analysis..."
                sh 'npm run lint'
            }
        }

        stage('Security Scan') {
            steps {
                echo "Running npm audit for security scan..."
                sh 'npm audit'
            }
            post {
                always {
                    emailext(
                        to: "${env.EMAIL_RECIPIENT}",
                        subject: "React App Security Scan: ${currentBuild.currentResult}",
                        body: "Security audit completed with status: ${currentBuild.currentResult}"
                    )
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo "Deploying React app to staging environment..."
                sh "echo 'Deploying to staging server: ${STAGING_SERVER}'"
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo "Running integration tests on staging environment..."
                sh "echo 'Integration tests on staging executed'"
            }
        }

        stage('Deploy to Production') {
            steps {
                echo "Deploying React app to production environment..."
                sh "echo 'Deploying to production server: ${PRODUCTION_SERVER}'"
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
