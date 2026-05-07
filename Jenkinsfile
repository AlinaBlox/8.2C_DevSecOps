ChatGPT


Uploaded image
I am trying to set up a jenkins pipeline with email sent, what would you say is going wrong here? because it does not get sent.

Thought for 5s
Your code looks mostly correct, so the problem is likely not this post block.

The main things I would check are:

Email Extension plugin settings

emailext(...) uses Extended E-mail Notification, not just normal Jenkins email settings.

Go to:
Manage Jenkins → System → Extended E-mail Notification

Make sure Gmail SMTP is configured there too.

SMTP settings
Use:

SMTP server: smtp.gmail.com
SMTP port: 587
Use TLS: ticked
Username: your full Gmail address
Password: your Gmail app password
Test email from Jenkins settings
In the same email section, use Test configuration by sending test e-mail.
If that does not work, the pipeline email will not work either.

Try a simpler email first
Replace your emailext temporarily with:

post {
    always {
        emailext(
            to: 'alinabloxsom@gmail.com',
            subject: 'Jenkins test email',
            body: 'This is a test email from Jenkins.'
        )
    }
}
If this works, then the issue may be with attachLog: true.

Check the Jenkins console output
After the build runs, look near the bottom for something like:

Sending email to:
or an SMTP error.

So I would say: your pipeline syntax is probably okay. The issue is most likely the Jenkins SMTP / Extended Email configuration, or Gmail app password setup.


the test email was sent correctly, so all correct from that end

Then the issue is most likely inside the pipeline, not Gmail.

Try changing this:

subject: "Jenkins Test Stage Result: ${currentBuild.currentResult}",
to this:

subject: "Jenkins Test Stage Result: ${currentBuild.result ?: 'SUCCESS'}",
Because during post { always { ... } }, currentBuild.currentResult can behave oddly depending on the build state.

Use this full version:

post {
    always {
        emailext(
            to: 'alinabloxsom@gmail.com',
            subject: "Jenkins Test Stage Result: ${currentBuild.result ?: 'SUCCESS'}",
            body: 'The Run Tests stage has completed. See attached build log.',
            attachLog: true
        )
    }
}
Also check that this post block is placed inside the main pipeline { } block, after stages { }, not inside one individual stage unless that is intentional.


Uploaded image
this is my current code, can we change it do what the official jenkins setup shows: pipeline {
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
                    emailext(
                        to: 'alinabloxsom@gmail.com',
                        subject: "Jenkins Test Stage Result: ${currentBuild.currentResult}",
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
                        subject: "Jenkins Security Scan Result: ${currentBuild.currentResult}",
                        body: 'The Security Scan stage has completed. See attached build log.',
                        attachLog: true
                    )
                }
            }
        }
    }
}

Yes. Change your emailext(...) blocks to the official format without brackets around the parameters.

Use this:

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
