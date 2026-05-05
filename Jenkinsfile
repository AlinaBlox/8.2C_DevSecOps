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
                echo 'Tool: Mocha'
                bat 'npm test || exit /b 0'
            }
            post {
                always {
                    emailext(
                        to: 'alinabloxsom@gmail.com',
                        subject: 'Jenkins Test Stage Result: ${currentBuild.currentResult}',
                        body: 'The Run Tests stage has completed. See attached build log.',
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
                always {
                    emailext(
                        to: 'alinabloxsom@gmail.com',
                        subject: 'Jenkins Security Scan Result: ${currentBuild.currentResult}',
                        body: 'The Security Scan stage has completed. See attached build log.',
                        attachLog: true
                    )
                }
            }
        }
    }
}