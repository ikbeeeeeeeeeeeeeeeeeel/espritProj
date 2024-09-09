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

        stage('Code Quality Test') {
            steps {
                script {
                    sh 'chmod +x ./mvnw'
                }
                withSonarQubeEnv('MySonarQube') {
                    sh 'mvn sonar:sonar -Dsonar.host.url=http://172.17.0.3:9000 -Dsonar.login=squ_4de4f2dbcd7eaa3bd7ef096e02c163fb201db941'
                }
            }
        }

        stage('Prepare for Nexus Deployment') {
            steps {
                echo "Preparing for Nexus deployment"
                script {
                    def settingsFile = '/usr/share/maven/conf/settings.xml'
                    if (fileExists(settingsFile)) {
                        sh "cat ${settingsFile}"
                    } else {
                        error "Maven settings.xml file not found at ${settingsFile}"
                    }
                }
            }
        }

        stage('Deploy to Nexus') {
            steps {
                echo "Deploying to Nexus"
                withCredentials([usernamePassword(credentialsId: 'nexus-ikbel', usernameVariable: 'NEXUS_USERNAME', passwordVariable: 'NEXUS_PASSWORD')]) {
                    script {
                        // Ensure Maven is configured properly
                        def settingsFile = '/usr/share/maven/conf/settings.xml'
                        if (fileExists(settingsFile)) {
                            echo "Maven settings.xml file found at ${settingsFile}"
                        } else {
                            error "Maven settings.xml file not found at ${settingsFile}"
                        }
                        
                        // Run Maven deploy command with credentials
                        try {
                            sh '''
                            # Run Maven deploy with credentials
                            mvn deploy \
                                -DaltDeploymentRepository=deploymentRepo::default::http://172.17.0.4:8081/repository/maven-releases/ \
                                -Dusername=${NEXUS_USERNAME} \
                                -Dpassword=${NEXUS_PASSWORD}
                            '''
                        } catch (Exception e) {
                            echo "Deployment failed: ${e.message}"
                            currentBuild.result = 'FAILURE'
                            throw e
                        }
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
