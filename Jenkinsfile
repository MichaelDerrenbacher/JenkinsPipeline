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
