pipeline { 
    environment {
    user = "turik207"
    repo = "app_helm"
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
        always { 
            slackSend message: "Start chenking for leaks - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
        }
        success {
            // slackSend message: "Gitleaks didn't find any secret leaks - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
            // slackSend message: "${custom_msg()}"
            slackSend (channel: '#jenkins_gitleaks', color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
    }
        failure {
            // slackSend message: "${custom_msg()}"
            slackSend (channel: '#jenkins_gitleaks', color: '#FF0000', message: ${custom_msg()} )
      }
   }
}
def custom_msg()
{
  def JENKINS_URL= "${env.BUILD_URL}"
  def JOB_NAME = env.JOB_NAME
  def BUILD_ID= env.BUILD_ID
  def FAIL_REASON= "there are passwords or secrets in $repo repo"
  def JENKINS_LOG= " FAILED: Job [${env.JOB_NAME}] Reason:${FAIL_REASON} Logs path: ${JENKINS_URL}/job/${JOB_NAME}/${BUILD_ID}/consoleText "
  return JENKINS_LOG
}