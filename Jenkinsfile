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
                    sh 'mvn sonar:sonar -Dsonar.host.url=http://172.17.0.3:9000 -Dsonar.login=squ_7b146ef04a44469a3b785d1b8db2bdc537b61584'
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
