pipeline {
    agent any
    
    //options {}
    
    stages {
        stage('Run Container') { 
            steps {
                bat "docker run -d -i  alpine cat"
            }
        }
        stage('View Container') { 
            steps {
                bat "docker ps"
            }
        }
    }
}
