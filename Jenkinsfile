pipeline {
    agent any 
    stages {
        stage('Build') { 
            steps {
                bat "echo Hey"
            }
        }
        stage('Test') { 
            steps {
                bat "echo There"
            }
        }
        stage('Deploy') { 
            steps {
                bat "echo Friend"
            }
        }
    }

    post {
        success {
            bat "echo Success!"
        }
    }
}
