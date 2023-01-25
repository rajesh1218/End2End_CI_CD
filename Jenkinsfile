pipeline {
    agent 
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven"
    }
    stages {
        stage('Git Checkout') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', url: 'https://github.com/rajesh1218/End2End_CI_CD.git'   
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
    }
}