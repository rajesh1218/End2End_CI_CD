pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven"
    }

    stages {
        stage('Git Checkout') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', url: 'https://github.com/jallu225/demo-counter-app.git'
                
            }

        }
        stage('UNIT Test'){
            steps {
                script {
                     sh "mvn test"
                }
            }
        }
        stage('Integration Testing'){
            steps {
                script {
                    sh "mvn verify -DskipUnitTests"
                }
            }
        }
        stage('Maven Build'){
            steps {
                script {
                    sh "mvn clean install"
                }
            }
        }
        stage('Static code analysis'){
            
            steps{
                
                script{
                    
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                        
                        sh "mvn clean package sonar:sonar"
                    }
                }
                    
                }
        }
        stage('Quality gate Status'){
            steps {
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                }
            }
        }
        stage('Upload artifacts to nexus'){
            steps {
                script {
                    nexusArtifactUploader artifacts: 
                    [
                        [
                            artifactId: 'springboot', 
                            classifier: '', 
                            file: 'target/Uber.jar', 
                            type: 'jar'
                            ]
                    ], 
                    credentialsId: 'Nexus-Cred', 
                    groupId: 'com.example', 
                    nexusUrl: '20.169.245.6:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'demoapp-releases', 
                    version: '1.0.0'
                }
            }
        }
    }
}