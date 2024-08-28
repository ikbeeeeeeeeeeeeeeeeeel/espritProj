pipeline {
    agent any

    tools {
        maven 'Maven_Home' // Add this if Maven is required
    }

    environment {
        SONARQUBE_URL = 'http://localhost:9000'
        SONARQUBE_SCANNER_HOME = tool name: 'SonarQube Scanner'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                withSonarQubeEnv('SonarQube') {
                    sh "${SONARQUBE_SCANNER_HOME}/bin/sonar-scanner"
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Deploy to Nexus') {
            steps {
                echo 'Deploying the application to Nexus...'
                sh 'mvn deploy -Dmaven.test.skip=true'
            }
        }
    }
}
