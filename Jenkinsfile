node {
    stage('Git Checkout'){
        git branch: 'main', url: 'https://github.com/rajesh1218/End2End_CI_CD.git'
    }
    stage('UNIT Test'){
        sh "mvn test"
    }
    stage('Integration Testing'){
        sh "mvn verify -DskipUnitTests"
     }
     stage('Maven Build'){
        sh "mvn clean install"
     }
    stage('sonar code analysis'){
        withSonarQubeEnv(credentialsId: 'sonar-token') {
           sh "mvn clean package sonar:sonar"
         }
    }
} 
