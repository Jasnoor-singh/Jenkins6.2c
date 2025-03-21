pipeline {
    agent any

    // Define environment variables for your servers and email.
    // These values should be updated with your actual staging/production addresses and email.
    environment {
        STAGING_SERVER = "ubuntu@ec2-16-170-159-223.eu-north-1.compute.amazonaws.com"       // Replace with your staging server
        PRODUCTION_SERVER = "ubuntu@<production-instance-public-dns>"   // Replace with your production server
        EMAIL_RECIPIENT = "singhjasnoor618@gmail.com"            // Replace with your email address
    }

    stages {

        stage('Checkout') {
            steps {
                // Use the default SCM configured in Jenkins.
                // Assumes that your GitHub token is set up in the Jenkins job's SCM configuration.
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
                // Using Maven to compile and package the code.
                sh 'mvn clean package'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                sh 'mvn test'
            }
            post {
                always {
                    // Send email notification after tests stage.
                    // Jenkins Email Extension Plugin should be configured in Jenkins.
                    mail to: "${env.EMAIL_RECIPIENT}",
                         subject: "Test Stage: ${currentBuild.currentResult}",
                         body: "Unit and Integration tests completed with status: ${currentBuild.currentResult}",
                         attachLog: true
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Running code analysis using SonarQube...'
                // Assumes you have configured a SonarQube server in Jenkins with the name 'SonarQube'
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running security scan using OWASP Dependency-Check...'
                // Adjust this command if necessary for your project.
                sh 'dependency-check.sh --project MyApp --scan .'
            }
            post {
                always {
                    // Send email notification after security scan stage.
                    mail to: "${env.EMAIL_RECIPIENT}",
                         subject: "Security Scan: ${currentBuild.currentResult}",
                         body: "Security scan completed with status: ${currentBuild.currentResult}",
                         attachLog: true
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying application to staging environment...'
                // Deploy the built JAR file to the staging server.
                // Since SSH keys are already configured in Jenkins, no credentials block is needed.
                sh "scp target/*.jar ${STAGING_SERVER}:/home/ubuntu/app.jar"
                sh "ssh ${STAGING_SERVER} 'nohup java -jar /home/ubuntu/app.jar > /dev/null 2>&1 &'"
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                // This is a simple example. You can replace it with a more robust integration test suite.
                sh 'curl -I http://staging.example.com:8080'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying application to production environment...'
                // Deploy the built JAR file to the production server.
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
