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
    }
}
