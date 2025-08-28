pipeline {
    agent any

    environment {
        REGISTRY = "docker.io"
        IMAGE_NAME = "jay24666/business-mgmt-app"
        SONAR_HOST_URL = "http://54.252.16.191:9000"
    }

    stages {

        stage("Build Code") {
            steps {
                sh "mvn clean install -DskipTests"
            }
        }

        stage("Run Code Scanning") {
            steps {
                script {
                    // resolve the Sonar Scanner installation path
                    def scannerHome = tool name: 'sonar-scanner'

                        withSonarQubeEnv('sonar-local') {
                 sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=business-mgmt-app \
                            -Dsonar.projectName=business-mgmt-app \
                            -Dsonar.sources=src \
                            -Dsonar.java.binaries=target/classes
                        """
                    }
                }
            }
        }

    }
}