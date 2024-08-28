pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out code from Git repository"
                checkout scm
            }
        }

        stage('MVN Build') {
            steps {
                echo "Running Maven build"
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONARQUBE_URL = 'http://localhost:9000' // Update if needed
            }
            steps {
                echo "Running SonarQube analysis"
                withSonarQubeEnv('MySonarQube') { // Adjust 'MySonarQube' to your SonarQube server name in Jenkins
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                echo "Waiting for SonarQube quality gate"
                waitForQualityGate abortPipeline: true
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}
