pipeline {
    agent any

    triggers {
        githubPush()
    }

    environment {
        // Update these with your actual server addresses and email.
        STAGING_SERVER     = "ubuntu@ec2-16-170-159-223.eu-north-1.compute.amazonaws.com"
        PRODUCTION_SERVER  = "ubuntu@<production-instance-public-dns>"
        EMAIL_RECIPIENT    = "singhjasnoor618@gmail.com"
    }

    stages {

        stage('Checkout') {
            steps {
                // Uses Jenkins's SCM settings with your GitHub token.
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies for React project...'
                // Use the NodeJS tool configured in Jenkins.
                // Ensure that "NodeJS" is configured in Global Tool Configuration.
                def nodeHome = tool name: 'NodeJS', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
                sh "${nodeHome}/bin/npm install"
            }
        }

        stage('Build') {
            steps {
                echo 'Building the React application...'
                def nodeHome = tool name: 'NodeJS', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
                sh "${nodeHome}/bin/npm run build"
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                def nodeHome = tool name: 'NodeJS', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
                // Adjust the test command if needed.
                sh "${nodeHome}/bin/npm test"
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
                def nodeHome = tool name: 'NodeJS', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
                // Example ESLint command; adjust according to your configuration.
                sh "${nodeHome}/bin/npm run lint"
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running security scan (e.g., npm audit)...'
                def nodeHome = tool name: 'NodeJS', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
                sh "${nodeHome}/bin/npm audit"
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
                // For real deployment, use scp/ssh commands.
                sh "echo 'Deploying app to staging server: ${STAGING_SERVER}'"
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                // For example, checking HTTP response from the staging server.
                sh "echo 'Simulating integration tests on staging'"
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production environment...'
                // For real deployment, use scp/ssh commands.
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
