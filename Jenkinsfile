pipeline { 
    environment { 
    repo = "app_helm"
    registry = "turik207/$repo"
    
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
                sh 'git clone https://github.com/"$registry".git && cd $repo  && gitleaks detect -v'
            }
        }

    }
}