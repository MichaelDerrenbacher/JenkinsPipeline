pipeline {
    agent any
    
    options {
        timestamps()
    }
    
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
    post {
        always {
            bat "docker stop ${ContainerName}"
            bat "docker rm ${ContainerName}"
            ERROR HERE 
        }
    }
}
