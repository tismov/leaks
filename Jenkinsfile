pipeline { 
    environment { 
    registry = "turik207/kube-deploy" 
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
                sh 'git clone https://github.com/"$registry".git && cd kube-deploy  && gitleaks detect -v'
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