pipeline {
    agent any 
    stages {
        stage('Build') { 
            steps {
                bat "echo Hi There"
            }
        }
        stage('Test') { 
            steps {
                bat "echo My"
            }
        }
        stage('Deploy') { 
            steps {
                bat "echo Friends"
            }
        }
    }

    post {
        success {
            bat "echo Success!"
        }
    }
}
