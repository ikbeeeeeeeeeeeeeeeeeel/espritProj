pipeline {
    agent any

    stages {
        stage('Code Recuperation') {
            steps {
                checkout scm
            }
        }

        stage('Remove old release') {
            steps {               
                sh 'mvn clean'
            }
        }

        stage('Compiling') {
            steps {               
                sh 'mvn compile'
            }
         } 

         stage('Code quality test') {
            steps { 
                script {
                    sh 'chmod +x ./mvnw'
                }
                withSonarQubeEnv(installationName: 'MySonarQube') {
                    sh './mvnw org.sonarsource.scanner.maven:sonar-maven-plugin:3.8.0.2131:sonar'
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
