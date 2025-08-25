pipeline {
    agent any

    environment {
        REGISTRY = "docker.io"
        IMAGE_NAME = "jay24666/business-mgmt-app"
        SONAR_HOST_URL = "http://sonarqube.local"
    }

    stages {
        stage("Build Code") {
            steps {
                sh "mvn clean install -DskipTests"
            }
        }

    }
}