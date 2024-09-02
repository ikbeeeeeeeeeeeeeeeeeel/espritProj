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
        

     stage('Code quality test') {
            steps { 
                script {
                    sh 'chmod +x ./mvnw'
                }
                withSonarQubeEnv(installationName: 'SonarQube Scanner') {
                    sh './mvnw org.sonarsource.scanner.maven:sonar-maven-plugin:3.3.0.1492:SonarQube Scanner'
                }
            }
        }  
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}
