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

        stage('Packaging into jar') {
            steps {
                // Package the application
                sh 'mvn package'
            }
        }
                stage('SonarQube Analysis') {
          steps {
            script {
            def scannerHome = tool 'MySonarScanner'
            withSonarQubeEnv('MySonarServer') {
              sh "mvn clean compile sonar:sonar " +
                "-Dsonar.projectKey=AutoSCMJava " +
                "-Dsonar.host.url=${env.SONAR_HOST_URL} " +
                "-Dsonar.login=${env.SONAR_TOKEN} " +
                "-Dsonar.java.binaries=target/classes"
               }
            }
          }
        }
        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
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
