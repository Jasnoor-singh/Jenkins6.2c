// 1. Define job properties (the GitHub push trigger) before the pipeline block
properties([
    pipelineTriggers([
        // This is equivalent to githubPush()
        [$class: 'GitHubPushTrigger']
    ])
])

pipeline {
    agent any

    // Environment variables for staging, production, and email
    environment {
        STAGING_SERVER     = "ubuntu@ec2-16-170-159-223.eu-north-1.compute.amazonaws.com"
        PRODUCTION_SERVER  = "ubuntu@<production-instance-public-dns>"
        EMAIL_RECIPIENT    = "singhjasnoor1421@gmail.com"
    }

    stages {

        stage('Checkout') {
            steps {
                // Uses Jenkins’s SCM settings (with your GitHub token).
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Simulating build step... (No actual project here)'
                // If you had a Maven project, for example:  sh 'mvn clean package'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Simulating unit and integration tests...'
            }
            post {
                always {
                    emailext(
                        to: "${env.EMAIL_RECIPIENT}",
                        subject: "Unit & Integration Tests: ${currentBuild.currentResult}",
                        body: "Simulated tests completed with status: ${currentBuild.currentResult}"
                    )
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Simulating code analysis (SonarQube, etc.)'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Simulating security scan (OWASP Dependency-Check, etc.)'
            }
            post {
                always {
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
                echo 'Simulating deployment to staging...'
                sh "echo 'Would SCP or SSH to ${STAGING_SERVER}'"
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Simulating integration tests on staging environment...'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Simulating deployment to production...'
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
