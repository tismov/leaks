pipeline { 
    environment {
    user = "turik207"
    repo = "kube-deploy"
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
                sh 'git clone https://github.com/"$registry".git && cd ./$repo && gitleaks detect -v'
            }
        }
    }
    post {
        failure {
            slackSend "Build Started - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
      }
   }
}