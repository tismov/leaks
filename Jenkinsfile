pipeline { 
    environment {
    user = "turik207"
    repo = "drone-test"
    registry = "$user/$repo"
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
                sh 'git clone https://github.com/"$registry".git && gitleaks detect -v ./$repo'
            }
        }
    }
}