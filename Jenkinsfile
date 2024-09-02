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
            steps {
                withCredentials([string(credentialsId: 'mahsem', variable: 'SONAR_TOKEN')]) { 
                    withSonarQubeEnv('MySonarQube') { 
                        sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:6.1.0.4477:sonar -Dsonar.host.url=$SONARQUBE_URL -Dsonar.login=$SONAR_TOKEN'
                    }
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
