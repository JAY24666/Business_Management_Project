pipeline {
    agent any

    environment {
        REGISTRY = "docker.io"
        IMAGE_NAME = "jay24666/business-mgmt-app"
        SONAR_HOST_URL = "http://3.106.213.149:9000"
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

        stage ("Check Quality Gate") {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

         stage("Upload Artifacts") {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: 'nexus:8081',
                    groupId: 'com.business',
                    version: '0.0.1-SNAPSHOT',   // must match POM
                    repository: 'maven-snapshots',  // snapshot repo
                    credentialsId: 'jenkins-nexus',
                    artifacts: [
                        [artifactId: 'BusinessProject',    // must match POM
                        classifier: '',
                        file: 'target/BusinessProject-0.0.1-SNAPSHOT.jar',
                        type: 'jar']
                    ]
                )
            }
        }

    }
}