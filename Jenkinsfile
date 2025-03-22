pipeline {
    agent any

    // Automatically trigger this pipeline whenever there's a push to the GitHub repository.
    triggers {
        githubPush()
    }

    environment {
        // Example: If you deploy somewhere, put the server addresses here.
        STAGING_SERVER    = "ubuntu@ec2-16-170-159-223.eu-north-1.compute.amazonaws.com"
        PRODUCTION_SERVER = "ubuntu@<production-instance-public-dns>"
        EMAIL_RECIPIENT   = "singhjasnoor1421@gmail.com"
    }

    stages {

        stage('Checkout') {
            steps {
                // Pull code from GitHub using the job's SCM configuration (GitHub token).
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies for React project...'
                // Assumes package.json is in the repo root. 
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the React application...'
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Your React test command:
                sh 'npm test'
            }
            post {
                always {
                    emailext (
                        to: "${env.EMAIL_RECIPIENT}",
                        subject: "React App Tests: ${currentBuild.currentResult}",
                        body: "Tests finished with status: ${currentBuild.currentResult}"
                    )
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Running code analysis... (e.g., ESLint)'
                // Example: ESLint check
                // sh 'npm run lint'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running security scan... (e.g., npm audit)'
                // Example: npm audit
                // sh 'npm audit'
            }
            post {
                always {
                    emailext (
                        to: "${env.EMAIL_RECIPIENT}",
                        subject: "Security Scan: ${currentBuild.currentResult}",
                        body: "Security scan finished with status: ${currentBuild.currentResult}"
                    )
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging environment...'
                // Example simulation: 
                sh "echo 'Would SCP or SSH to ${STAGING_SERVER}'"
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging environment...'
                // For real integration tests: 
                // sh 'curl -I http://<staging-instance-public-ip>:3000'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production environment...'
                // Example simulation:
                sh "echo 'Would SCP or SSH to ${PRODUCTION_SERVER}'"
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
