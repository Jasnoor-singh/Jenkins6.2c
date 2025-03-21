pipeline {
    agent any

    environment {
        // Update these with your actual staging/production server addresses.
        // For example, using your EC2 instance's public DNS:
        STAGING_SERVER = "ubuntu@ec2-16-170-159-223.eu-north-1.compute.amazonaws.com"
        PRODUCTION_SERVER = "ubuntu@<production-instance-public-dns>"
        // Update with your notification email address.
        EMAIL_RECIPIENT = "your-email@example.com"
    }

    stages {

        stage('Checkout') {
            steps {
                // Use the built-in SCM; this assumes your job is set up with the GitHub token.
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Simulating build step... (No actual build command since there is no project)'
                // If you had a Maven project, you might run:
                // sh 'mvn clean package'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Simulating unit and integration tests...'
                // Replace with actual test commands if available.
                // sh 'mvn test'
            }
            post {
                always {
                    emailext (
                        to: "${env.EMAIL_RECIPIENT}",
                        subject: "Unit and Integration Tests: ${currentBuild.currentResult}",
                        body: "Simulated unit and integration tests completed with status: ${currentBuild.currentResult}"
                    )
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Simulating code analysis (e.g., SonarQube scan)...'
                // withSonarQubeEnv('SonarQube') { sh 'mvn sonar:sonar' }
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Simulating security scan (e.g., OWASP Dependency-Check)...'
                // Replace with an actual security scan command if needed.
                // sh 'dependency-check.sh --project MyApp --scan .'
            }
            post {
                always {
                    emailext (
                        to: "${env.EMAIL_RECIPIENT}",
                        subject: "Security Scan: ${currentBuild.currentResult}",
                        body: "Simulated security scan completed with status: ${currentBuild.currentResult}"
                    )
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Simulating deployment to staging environment...'
                // Simulate deployment. If using SCP/SSH, you might run commands like:
                // sh "scp target/*.jar ${STAGING_SERVER}:/home/ubuntu/app.jar"
                // sh "ssh ${STAGING_SERVER} 'nohup java -jar /home/ubuntu/app.jar > /dev/null 2>&1 &'"
                sh "echo 'Deploying app to staging server: ${STAGING_SERVER}'"
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Simulating integration tests on staging environment...'
                // You could simulate this by, for example, checking an HTTP response:
                // sh 'curl -I http://<staging-instance-dns>:8080'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Simulating deployment to production environment...'
                // Simulate production deployment. Replace with real commands as needed.
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
