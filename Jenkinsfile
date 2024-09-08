pipeline {
    agent any
    tools {
        maven 'MAVEN_HOME'
        jdk 'JAVA_HOME'
    }

    environment{
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "172.17.0.4:8081"
        NEXUS_REPOSITORY = "nexus-ikbel"
        NEXUS_CREDENTIAL_ID = "nexus-ikbel"
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
        stage('Deploy to Nexus') {
            steps {
                script {
                    // Use Maven to deploy artifacts to Nexus
                    withCredentials([usernamePassword(credentialsId: "${NEXUS_CREDENTIAL_ID}", usernameVariable: 'NEXUS_USERNAME', passwordVariable: 'NEXUS_PASSWORD')]) {
                        sh '''
                        mvn deploy -DskipTests \
                        -DaltDeploymentRepository=${NEXUS_REPOSITORY}::default::${NEXUS_PROTOCOL}://${NEXUS_URL}/repository/${NEXUS_REPOSITORY}/ \
                        -Dusername=${NEXUS_USERNAME} \
                        -Dpassword=${NEXUS_PASSWORD}
                        '''
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
