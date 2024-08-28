pipeline {
    agent any

    stages {
        stage('GIT') {
            steps {
                echo "Getting Project from git"
                // Add actual Git commands here
            }
        }
        
        stage('MVN CLEAN') {
            steps {
                sh 'mvn clean'
            }
        }
    
        stage('MVN COMPILE') {
            steps {
                sh 'mvn compile'
            }
        }
            
        stage('MVN SonarQube') {
            steps {
                // Add SonarQube scan commands here
            }
        }
    }
}
