pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'sonar'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                bat './gradlew test'
            }
        }

        stage('Code Analysis (Jacoco)') {
            steps {
                bat './gradlew jacocoTestReport'
            }
        }

        stage('Build') {
            steps {
                bat './gradlew build'
            }
        }

        stage('Deploy') {
            steps {
                bat './gradlew publish'
            }
        }
    }

    post {
        success {
            mail to: 'ma_lattari@esi.dz',
                 subject: 'Jenkins Pipeline SUCCESS',
                 body: '''
The Jenkins pipeline completed successfully.

- Tests: OK
- Code Quality: PASSED
- Build: OK
- Deploy: SUCCESS
'''

            script {
                withCredentials([string(credentialsId: 'SLACK_WEBHOOK', variable: 'SLACK_WEBHOOK_URL')]) {
                    bat '''
curl -X POST -H "Content-type: application/json" ^
--data "{\\"text\\":\\"✅ SUCCESS - %JOB_NAME% #%BUILD_NUMBER% %BUILD_URL%\\"}" ^
%SLACK_WEBHOOK_URL%
'''
                }
            }
        }
    }
}
