pipeline { 
    environment { 
    registry = "turik207/app_helm" 
    }
    agent {
        docker { 
            image 'zricethezav/gitleaks' 
            args '--entrypoint=""'
        }
    }
    stages { 
        stage('check for leaks') {
            steps {
                sh 'git clone https://github.com/"$registry".git && cd app_helm && gitleaks detect -v'
                // sh 'gitleaks detect -v'
            }
        }
        stage('Cleaning up') { 
            steps { 
                sh "docker rmi $registry" 
            }
        } 

    }
}