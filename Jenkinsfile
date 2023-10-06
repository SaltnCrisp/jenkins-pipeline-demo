pipeline {
    agent any

    tools {
        maven 'Maven3.9.5'
        jdk 'JDK17'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                echo 'Testing...'
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/test-*.xml'
                }
                failure {
                    echo 'Sending failure email...'
                    emailext(
                        to: 'saltncrisp0102@gmail.com',
                        subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                        body: """<p>FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                                 <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
                        attachLog: true
                    )
                }
            }
        }
        stage('Code Analysis') {
            steps {
                echo 'Analyzing code...'
                // Integrate code analysis tool here
            }
        }
        stage('Security Scan') {
            steps {
                echo 'Scanning for security vulnerabilities...'
                // Integrate your security scanning tool here
            }
            post {
                failure {
                    echo 'Sending failure email...'
                    emailext(
                        to: 'saltncrisp0102@gmail.com',
                        subject: "SECURITY SCAN FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                        body: """<p>SECURITY SCAN FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                                 <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
                        attachLog: true
                    )
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging...'
                // Add deployment to staging commands/scripts here
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                // Add integration tests for staging here
            }
        }
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production...'
                // Add deployment to production commands/scripts here
            }
        }
    }
}
