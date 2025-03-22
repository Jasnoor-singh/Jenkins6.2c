pipeline {
    agent any

    // Automatically trigger on every GitHub push.
    triggers {
        githubPush()
    }

    environment {
        // These can be updated to reflect your environment if needed.
        STAGING_SERVER    = "ubuntu@ec2-16-170-159-223.eu-north-1.compute.amazonaws.com"
        PRODUCTION_SERVER = "ubuntu@<production-instance-public-dns>"
        EMAIL_RECIPIENT   = "singhjasnoor1421@gmail.com"
    }

    stages {

        stage('Checkout') {
            steps {
                // Pull code from your GitHub repo using the job's SCM config.
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Simulating dependency installation... (No real commands here)'
                // e.g., "sh 'npm install'" if you had a Node project
            }
        }

        stage('Build') {
            steps {
                echo 'Simulating build step...'
                // e.g., "sh 'mvn clean package'" or "sh 'npm run build'"
            }
        }

        stage('Test') {
            steps {
                echo 'Simulating test step...'
                // e.g., "sh 'mvn test'" or "sh 'npm test'"
            }
            post {
                always {
                    // Send an email notification at the end of the test stage
                    emailext(
                        to: "${env.EMAIL_RECIPIENT}",
                        subject: "Test Stage: ${currentBuild.currentResult}",
                        body: "Tests completed with status: ${currentBuild.currentResult}"
                    )
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Simulating code analysis (SonarQube/ESLint/etc.)...'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Simulating security scan (OWASP Dependency-Check/npm audit/etc.)...'
            }
            post {
                always {
                    // Send an email notification at the end of the security scan
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
                echo 'Simulating deployment to staging environment...'
                // e.g., "sh 'scp build/* ${STAGING_SERVER}:/var/www/myapp'"
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Simulating integration tests on staging environment...'
                // e.g., "sh 'curl -I http://<staging-instance-ip>:3000'"
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Simulating final deployment to production...'
                // e.g., "sh 'scp build/* ${PRODUCTION_SERVER}:/var/www/myapp'"
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
