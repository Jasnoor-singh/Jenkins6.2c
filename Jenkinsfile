pipeline {
    agent any

    // Automatically trigger the pipeline on a GitHub push.
    triggers {
        githubPush()
    }

    // Use the NodeJS tool defined in Global Tool Configuration.
    tools {
        nodejs 'NodeJS'
    }

    environment {
        // Replace these with your actual server addresses.
        STAGING_SERVER    = "ubuntu@ec2-16-170-159-223.eu-north-1.compute.amazonaws.com"
        PRODUCTION_SERVER = "ubuntu@<production-instance-public-dns>"
        EMAIL_RECIPIENT   = "singhjasnoor1421@gmail.com"
    }

    stages {

        stage('Checkout') {
            steps {
                // Uses the Jenkins job's SCM settings (with your GitHub token).
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies for React project...'
                // With NodeJS installed via the tools block, npm is available.
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
                sh 'npm test'
            }
            post {
                always {
                    emailext(
                        to: "${env.EMAIL_RECIPIENT}",
                        subject: "React App Tests: ${currentBuild.currentResult}",
                        body: "Tests completed with status: ${currentBuild.currentResult}"
                    )
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Running code analysis (e.g., ESLint)...'
                // Uncomment and adjust if you have a lint script:
                // sh 'npm run lint'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running security scan (e.g., npm audit)...'
                sh 'npm audit'
            }
            post {
                always {
                    emailext(
                        to: "${env.EMAIL_RECIPIENT}",
                        subject: "Security Scan: ${currentBuild.currentResult}",
                        body: "Security scan completed with status: ${currentBuild.currentResult}"
                    )
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging environment...'
                // Simulate deployment. Replace with real commands if available.
                sh "echo 'Deploying app to staging server: ${STAGING_SERVER}'"
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging environment...'
                // For example, simulate integration tests by checking an HTTP response:
                // sh 'curl -I http://<staging-instance-public-ip>:3000'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production environment...'
                // Simulate production deployment. Replace with your real deployment commands.
                sh "echo 'Deploying app to production server: ${PRODUCTION_SERVER}'"
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
