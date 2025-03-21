pipeline {
    agent any

    environment {
        // Set your server addresses (update these with your actual staging/production server DNS/IP)
        STAGING_SERVER = "ubuntu@ec2-16-170-159-223.eu-north-1.compute.amazonaws.com"
        PRODUCTION_SERVER = "ubuntu@<production-instance-public-dns>"
        // Set your email recipient
        EMAIL_RECIPIENT = "singhjasnoor1421@gmail.com"
    }

    stages {

        stage('Checkout') {
            steps {
                // Checkout cod from GitHub using the Jenkins job’s SCM configuration (which uses your GitHub token)
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
                // Use Maven to compile and package your application
               // sh 'mvn clean package'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                sh 'mvn test'
            }
            post {
                always {
                    // Send email notification after tests without attaching the log
                    emailext (
                        subject: "Test Stage: ${currentBuild.currentResult}",
                        body: "Unit and Integration tests completed with status: ${currentBuild.currentResult}",
                        to: "${env.EMAIL_RECIPIENT}"
                    )
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Running code analysis using SonarQube...'
                // Assumes SonarQube is configured in Jenkins with the name 'SonarQube'
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running security scan using OWASP Dependency-Check...'
                // Adjust the dependency-check command for your project if necessary
                sh 'dependency-check.sh --project MyApp --scan .'
            }
            post {
                always {
                    emailext (
                        subject: "Security Scan: ${currentBuild.currentResult}",
                        body: "Security scan completed with status: ${currentBuild.currentResult}",
                        to: "${env.EMAIL_RECIPIENT}"
                    )
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying application to staging environment...'
                // Deploy the built artifact to the staging server
                sh "scp target/*.jar ${STAGING_SERVER}:/home/ubuntu/app.jar"
                sh "ssh ${STAGING_SERVER} 'nohup java -jar /home/ubuntu/app.jar > /dev/null 2>&1 &'"
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging environment...'
                // Replace with your integration test command as needed
                sh 'curl -I http://staging.example.com:8080'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying application to production environment...'
                sh "scp target/*.jar ${PRODUCTION_SERVER}:/home/ubuntu/app.jar"
                sh "ssh ${PRODUCTION_SERVER} 'nohup java -jar /home/ubuntu/app.jar > /dev/null 2>&1 &'"
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
