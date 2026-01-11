pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'SonarQube'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                sh './gradlew test'
            }
        }

        stage('Code Analysis (Jacoco)') {
            steps {
                sh './gradlew jacocoTestReport'
            }
        }

        stage('Code Quality (SonarQube)') {
            steps {
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    sh './gradlew sonarqube'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Build') {
            steps {
                sh './gradlew build'
            }
        }

        stage('Deploy') {
            steps {
                sh './gradlew publish'
            }
        }
    }
}
