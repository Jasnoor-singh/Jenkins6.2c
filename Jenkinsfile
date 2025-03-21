pipeline {
    agent any

    environment {
        STAGING_SERVER = "ubuntu@<staging-ec2-ip>"
        PRODUCTION_SERVER = "ubuntu@<production-ec2-ip>"
        SSH_CREDENTIALS_ID = "your-ssh-credentials-id"
        EMAIL_RECIPIENT = "singhjasnoor618@gmail.com"
    }

    stages {

        stage('Build') {
            steps {
                echo 'Building the application...'
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
                    mail bcc: '', body: "Unit and Integration Tests completed.\nStatus: ${currentBuild.currentResult}",
                         from: '', replyTo: '', subject: "Test Stage: ${currentBuild.currentResult}",
                         to: "${env.EMAIL_RECIPIENT}",
                         attachLog: true
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Performing code analysis with SonarQube...'
                withSonarQubeEnv('My SonarQube Server') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running security scan using OWASP Dependency-Check...'
                sh 'dependency-check.sh --project MyApp --scan ./'
            }
            post {
                always {
                    mail bcc: '', body: "Security Scan completed.\nStatus: ${currentBuild.currentResult}",
                         from: '', replyTo: '', subject: "Security Scan: ${currentBuild.currentResult}",
                         to: "${env.EMAIL_RECIPIENT}",
                         attachLog: true
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging environment...'
                sshagent([env.SSH_CREDENTIALS_ID]) {
                    sh "scp target/*.jar ${STAGING_SERVER}:/home/ubuntu/app.jar"
                    sh "ssh ${STAGING_SERVER} 'nohup java -jar /home/ubuntu/app.jar > /dev/null 2>&1 &'"
                }
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging server...'
                // Add your integration test script or command here
                sh 'curl -I http://<staging-ec2-ip>:8080'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production environment...'
                sshagent([env.SSH_CREDENTIALS_ID]) {
                    sh "scp target/*.jar ${PRODUCTION_SERVER}:/home/ubuntu/app.jar"
                    sh "ssh ${PRODUCTION_SERVER} 'nohup java -jar /home/ubuntu/app.jar > /dev/null 2>&1 &'"
                }
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
