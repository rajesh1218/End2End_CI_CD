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
                    def readPomVersion = readMavenPom file: 'pom.xml'
                    def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT") ? "demoapp-snapshot" : "demoapp-releases"
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
                    repository: nexusRepo, 
                    version: "${readPomVersion.version}"
                }
            }
        }
        stage('Docker Build'){
            steps {
                script {
                    sh "docker image build -t $JOB_NAME:v1.$BUILD_ID ."
                    sh "docker tag $JOB_NAME:v1.$BUILD_ID rajesh1218/$JOB_NAME:v1.$BUILD_ID"
                    sh "docker tag $JOB_NAME:v1.$BUILD_ID rajesh1218/$JOB_NAME:v1.latest"

                }
            }
        }
        stage('push image to dockerhub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'docker_pass', variable: 'dokcer_cred')]) {
                      sh "docker login -u rajesh1218 -p ${dokcer_cred}"
                      sh "docker push rajesh1218/$JOB_NAME:v1.$BUILD_ID"
                      sh "docker push rajesh1218/$JOB_NAME:v1.latest"
                }
                }
            }
        }
    }
}