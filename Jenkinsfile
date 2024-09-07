pipeline {
    agent any
    tools {
        maven 'MAVEN_HOME'
    }

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
                withSonarQubeEnv('MySonarQube') {
                    sh 'mvn sonar:sonar'
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
