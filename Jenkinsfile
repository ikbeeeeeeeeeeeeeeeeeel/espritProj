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
            def mvn = tool 'Default Maven';
            withSonarQubeEnv() {
              sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=espritProj -Dsonar.projectName='espritProj'"
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
