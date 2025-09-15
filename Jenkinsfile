pipeline {
    agent any

    tools {
        maven 'MyMaven' // Ensure this version is configured in Jenkins
        jdk 'MyJDK'      // Ensure this JDK version is configured in Jenkins
    }
    
    stages {
        stage('CodeCheckout from Github') {
            steps {
                // Checkout code from SCM
                checkout scm
            }
        }

        stage('Maven Build') {
            steps {
                // Build the project using Maven
                sh 'mvn clean install'
            }
        }

        stage('Maven Test') {
            steps {
                // Run unit tests
                sh 'mvn test'
            }
        }
                stage('SonarQube Analysis') {
            steps {
                script {
                    // This 'tool' refers to the Sonar Scanner tool that you configured in Jenkins
                    def scannerHome = tool 'MySonarScanner'  
                    withSonarQubeEnv('MySonarScanner') {
                        // Now run sonar scanner binary
                        sh "${scannerHome}/bin/sonar-scanner \
                           -Dsonar.projectKey=HotstarClone \
                           -Dsonar.sources=src \
                           -Dsonar.host.url=${env.SONAR_HOST_URL} \
                           -Dsonar.login=${env.SONAR_AUTH_TOKEN}"
                    }
                }
            }
        }

        stage('Packaging into jar') {
            steps {
                // Package the application
                sh 'mvn package'
            }
        }
    }

    post {
        always {
            // Clean up actions
            sh 'echo "Cleaning up..."'
        }

        success {
            // Actions on successful build
            echo 'Build succeeded!'
        }

        failure {
            // Actions on failed build
            echo 'Build failed!'
        }
    }
}
