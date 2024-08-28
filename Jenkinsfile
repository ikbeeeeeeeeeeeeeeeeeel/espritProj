pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout code from Git repository
                    checkout scm
                }
            }
        }

        stage('MVN Clean') {
            steps {
                script {
                    // Run Maven clean
                    sh 'mvn clean'
                }
            }
        }

        stage('MVN Compile') {
            steps {
                script {
                    // Run Maven compile
                    sh 'mvn compile'
                }
            }
        }

        stage('SonarQube Analysis') {
            environment {
                // Define SonarQube environment variables
                SONARQUBE_URL = 'http://localhost:9000' // Change if needed
                SONARQUBE_SCANNER = tool 'SonarQube Scanner' // Adjust to your SonarQube Scanner tool name
            }
            steps {
                script {
                    // Run SonarQube analysis
                    withSonarQubeEnv('MySonarQube') { // Adjust 'MySonarQube' to your SonarQube server name in Jenkins
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    // Wait for SonarQube quality gate
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        always {
            // Clean up actions or notifications
            echo 'Pipeline finished.'
        }
    }
}
