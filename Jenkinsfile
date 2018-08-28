pipeline {
    agent none 
    
    environment {
        ContainerName = "${JOB_NAME}-${BUILD_NUMBER}"
    }
    
    stages {
        stage('Docker') { 
            agent {
				docker {
					label 'master'
					image 'kbedel/jenkins-node-gulp:node10'
					args  '--privileged --name "${ContainerName}"'
				}
			}
            steps {
                bat '''
                    echo "Yo"
                '''
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
