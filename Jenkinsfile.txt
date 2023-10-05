pipeline {
    agent any

    tools {
        maven 'Maven3.6'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                sh 'mvn test'
            }
            post {
                success {
                    emailext (
                        to: 'saltncrisp0102@gmail.com',
                        subject: "SUCCESS: Tests passed for ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "All tests passed successfully. Check attached logs for details.",
                        attachLog: true
                    )
                }
                failure {
                    emailext (
                        to: 'saltncrisp0102@gmail.com',
                        subject: "FAILURE: Tests failed for ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "Some tests failed. Check attached logs for details.",
                        attachLog: true
                    )
                }
            }
        }

        stage('Code Analysis') {
            steps {
                sh 'mvn sonar:sonar'
            }
        }

        stage('Security Scan') {
            steps {
                sh 'mvn org.owasp:dependency-check-maven:check'
            }
            post {
                success {
                    emailext (
                        to: 'saltncrisp0102@gmail.com',
                        subject: "SUCCESS: Security scan passed for ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "Security scan passed successfully. Check attached logs for details.",
                        attachLog: true
                    )
                }
                failure {
                    emailext (
                        to: 'saltncrisp0102@gmail.com',
                        subject: "FAILURE: Security vulnerabilities found in ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "Security scan detected vulnerabilities. Check attached logs for details.",
                        attachLog: true
                    )
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                // This is a placeholder. Adjust based on your deployment strategy.
                echo 'Deploying to Staging...'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                // Another placeholder. Adjust based on your testing strategy for staging.
                echo 'Running integration tests on staging...'
            }
        }

        stage('Deploy to Production') {
            steps {
                // Placeholder. Adjust based on your deployment strategy.
                echo 'Deploying to Production...'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
    }
}
