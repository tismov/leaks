pipeline { 
    environment { 
    registry = "turik207/drone-test" 
    }

    // agent {
    //     docker { image 'zricethezav/gitleaks' }
    // }
    agent {
        docker { 
            image 'zricethezav/gitleaks' 
            args '--entrypoint=""'
        }
    }
    stages { 
        // stage('Cloning from my Github') { 
        //     steps { 
        //         git changelog: false, credentialsId: 'githubAccess', poll: false, url: 'git@github.com:turik207/$registry.git'
        //     }
        // }
        stage('check for leaks') {
            steps {
                sh 'git clone https://github.com/"$registry".git && cd drone-test && gitleaks detect -v'
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