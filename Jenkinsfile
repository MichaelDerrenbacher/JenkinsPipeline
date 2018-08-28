pipeline {
    agent any
    
    stages {
        stage('Docker') { 
            
            steps {
                bat "docker run -d -i alpine cat"
                bat "docker ps"
            }
        }
    }
}
