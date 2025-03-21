pipeline {
    agent any

    // Automatically trigger the pipeline when a push is made to GitHub.
    triggers {
        githubPush()
    }

    environment {
        // Set your server addresses.
        // Use your EC2 instance public DNS/IP. For example, if this instance is for staging:
        STAGING_SERVER = "ubuntu@ec2-16-170-159-223.eu-north-1.compute.amazonaws.com"
        // Replace <production-instance-public-dns> with your production server's address.
        PRODUCTION_SERVER = "ubuntu@<production-instance-public-dns>"
        // Set your notification email address.
        EMAIL_RECIPIENT = "singhjasnoor1421@gmail.com"
    }

    stages {

        stage('Checkout') {
            steps {
                // The built-in SCM configuration uses your GitHub token.
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
                // Replace with actual test commands if available:
                // sh 'mvn test'
            }
            post {
                always {
                    // Send an email notification with the result of the tests.
                    emailext(
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
                // If you had SonarQube configured, you might use:
                // withSonarQubeEnv('SonarQube') { sh 'mvn sonar:sonar' }
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Simulating security scan (e.g., OWASP Dependency-Check)...'
                // Replace with an actual security scan command if needed:
                // sh 'dependency-check.sh --project MyApp --scan .'
            }
            post {
                always {
                    // Send an email notification with the result of the security scan.
                    emailext(
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
                // Simulate deployment. In a real scenario, you might use SCP/SSH:
                // sh "scp target/*.jar ${STAGING_SERVER}:/home/ubuntu/app.jar"
                // sh "ssh ${STAGING_SERVER} 'nohup java -jar /home/ubuntu/app.jar > /dev/null 2>&1 &'"
                sh "echo 'Deploying app to staging server: ${STAGING_SERVER}'"
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Simulating integration tests on staging environment...'
                // For example, simulate by checking an HTTP response:
                // sh 'curl -I http://<staging-instance-dns>:8080'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Simulating deployment to production environment...'
                // Simulate production deployment. In a real scenario:
                // sh "scp target/*.jar ${PRODUCTION_SERVER}:/home/ubuntu/app.jar"
                // sh "ssh ${PRODUCTION_SERVER} 'nohup java -jar /home/ubuntu/app.jar > /dev/null 2>&1 &'"
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
