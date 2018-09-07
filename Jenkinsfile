pipeline {
    agent any
    
    environment {
        ContainerName = "Container-${BUILD_NUMBER}"
    }
    
    stages {
        stage('Run Container') { 
            steps {
                bat "docker run --name ${ContainerName} -d -i  alpine cat"
            }
        }
        stage('View Container') { 
            steps {
                bat "docker ps"
            }
        }
    }
}
