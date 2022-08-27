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
                sh 'git clone https://github.com/"$registry".git && cd ./$repo && gitleaks detect -v'
            }
        }
    }
    post {
        success {
            // slackSend message: "Gitleaks didn't find any secret leaks - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
            slackSend message: "${custom_msg()}"
    }
        failure {
            slackSend message: "${custom_msg()}"
            // slackSend message: "Build failed - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
      }
   }
}
def custom_msg()
{
  def JENKINS_URL= "${env.BUILD_URL}"
  def JOB_NAME = env.JOB_NAME
  def BUILD_ID= env.BUILD_ID
  def FAIL_REASON= "tapildi"
  def JENKINS_LOG= " FAILED: Job [${env.JOB_NAME}] /n Reason:${FAIL_REASON} Logs path: ${JENKINS_URL}/job/${JOB_NAME}/${BUILD_ID}/consoleText "
  return JENKINS_LOG
}