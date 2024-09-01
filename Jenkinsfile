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
                SONARQUBE_URL = '172.17.0.1:9000' 
            }
            steps {
                echo "Running SonarQube analysis"
                withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) { 
                    withSonarQubeEnv('MySonarQube') { 
                        sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:4.0.0.4121:sonar -Dsonar.host.url=$SONARQUBE_URL -Dsonar.login=$SONAR_TOKEN'
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
