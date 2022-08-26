pipeline { 
    environment { 
    registry = "turik207/drone-test" 
    }

    agent {
        docker { image 'zricethezav/gitleaks' }
    }
    stages { 
        // stage('Cloning from my Github') { 
        //     steps { 
        //         git changelog: false, credentialsId: 'githubAccess', poll: false, url: 'git@github.com:turik207/$registry.git'
        //     }
        // }
        stage('check for leaks') {
            steps {
                sh 'git clone https://github.com/turik207/$registry && cd $registry && gitleaks detect -v'
            }
        }
        stage('Cleaning up') { 
            steps { 
                sh "docker rmi $registry" 
            }
        } 

    }
}