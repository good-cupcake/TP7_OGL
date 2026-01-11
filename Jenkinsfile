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
                bat './gradlew test'
            }
        }

        stage('Code Analysis (Jacoco)') {
            steps {
                bat './gradlew jacocoTestReport'
            }
        }

        stage('Code Quality (SonarQube)') {
            steps {
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    bat './gradlew sonarqube'
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
                bat './gradlew build'
            }
        }

        stage('Deploy') {
            steps {
                bat './gradlew publish'
            }
        }
    }
}
