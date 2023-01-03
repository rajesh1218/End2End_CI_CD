pipeline {
    agent any
    stages ('Git Checkout') {
        stage {
            steps {
                script{
                    git branch: 'main', url: 'https://github.com/jallu225/demo-counter-app.git'
                }
            }
        }
    }
}