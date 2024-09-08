pipeline {
    agent any
    tools {
        maven 'MAVEN_HOME'
        jdk 'JAVA_HOME'
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
        stage('JDK Test') {
            steps {
                echo "Testing JDK Installation"
                sh '''
                # Print Java version to verify JDK installation
                java -version

                # Print JAVA_HOME to ensure it's set correctly
                echo $JAVA_HOME
                '''
            }
        }
        
        stage('Code quality test') {
            steps { 
                script {
                    sh 'chmod +x ./mvnw'
                }
                withSonarQubeEnv('MySonarQube') {
                    sh 'mvn sonar:sonar -Dsonar.host.url=http://172.17.0.3:9000 -Dsonar.login=squ_cf113dfdc89053dde492519fbbba825e00af00d0'
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
