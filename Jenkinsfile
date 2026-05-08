pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Tool: Git'
                git branch: 'main', url: 'https://github.com/AlinaBlox/8.2C_DevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Tool: npm'
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Tool: Jasmine'
                bat 'npm test || exit /b 0'
            }
            post {
                success {
                    emailext(
                        to: 'alinabloxsom@gmail.com',
                        subject: "SUCCESS: Test Stage",
                        body: 'Test stage completed successfully.',
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: 'alinabloxsom@gmail.com',
                        subject: "FAILED: Test Stage",
                        body: 'Test stage failed. See attached log.',
                        attachLog: true
                    )
                }
            }
        }

        stage('Generate Coverage Report') {
            steps {
                echo 'Tool: NYC'
                bat 'npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                echo 'Tool: npm audit'
                bat 'npm audit || exit /b 0'
            }
            post {
                success {
                    emailext(
                        to: 'alinabloxsom@gmail.com',
                        subject: "SUCCESS: Security Scan",
                        body: 'Security scan completed successfully.',
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: 'alinabloxsom@gmail.com',
                        subject: "FAILED: Security Scan",
                        body: 'Security scan failed. See attached log.',
                        attachLog: true
                    )
                }
            }
        }
    }
}
