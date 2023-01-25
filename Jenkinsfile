node {
    stage('Git Checkout'){
        git branch: 'main', url: 'https://github.com/rajesh1218/demo-counter-app.git'
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
} 
