pipeline {
    // Trigger the pipeline automatically on GitHub pushes.
    triggers {
        githubPush()
    }

    environment {
        // Example environment variables for staging/production & emails.
        STAGING_SERVER    = "ubuntu@ec2-16-170-159-223.eu-north-1.compute.amazonaws.com"
        PRODUCTION_SERVER = "ubuntu@<production-instance-public-dns>"
        EMAIL_RECIPIENT   = "singhjasnoor1421@gmail.com"
    }

    stages {

        stage('Checkout') {
            steps {
                // Pulls your code from GitHub using Jenkins job's SCM config (GitHub token).
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies for React project...'
                // node:18-alpine Docker image includes npm by default.
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
                // Typical React test command
                sh 'npm test'
            }
            post {
                always {
                    emailext(
                        to: "${env.EMAIL_RECIPIENT}",
                        subject: "React App Tests: ${currentBuild.currentResult}",
                        body: "Tests finished with status: ${currentBuild.currentResult}"
                    )
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Running code analysis (e.g. ESLint)...'
                // Example command:
                // sh 'npm run lint'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running security scan (e.g. npm audit)...'
                // sh 'npm audit'
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
                // Placeholder for real deployment. For instance:
                // sh "scp -r build/* ${STAGING_SERVER}:/var/www/react-app"
                // sh "ssh ${STAGING_SERVER} 'sudo systemctl restart nginx'"
                sh "echo 'Would deploy to staging server: ${STAGING_SERVER}'"
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Performing integration tests on staging environment...'
                // Example: sh 'curl -I http://<staging-server-public-ip>:3000'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production environment...'
                // Placeholder for real production deployment.
                sh "echo 'Would deploy to production server: ${PRODUCTION_SERVER}'"
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
