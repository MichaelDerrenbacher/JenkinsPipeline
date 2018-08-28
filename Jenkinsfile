pipeline {
    agent any
    
    environment {
        ContainerName = "${JOB_NAME}-${BUILD_NUMBER}"
    }
    
    stages {
        stage('Docker') { 
            
            steps {
                bat "docker run -d -i --name "${ContainerName}" alpine cat"
                bat "docker ps"
            }
        }
    }
}

post{
    always {
        bat "docker stop ${ContainerName}"
        bat "docker rm ${ContainerName}"
    }  
}
