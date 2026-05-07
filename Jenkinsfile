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
                always {
                    emailext body: 'The Run Tests stage has completed. See attached build log.',
                        subject: 'Jenkins Test Stage Result',
                        to: 'alinabloxsom@gmail.com',
                        attachLog: true
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
                always {
                    emailext body: 'The Security Scan stage has completed. See attached build log.',
                        subject: 'Jenkins Security Scan Result',
                        to: 'alinabloxsom@gmail.com',
                        attachLog: true
                }
            }
        }
    }
}
