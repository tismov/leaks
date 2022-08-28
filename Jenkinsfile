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
                sh 'git clone https://github.com/"$registry".git && cd ./$repo && gitleaks detect -v -r .github/gitleaks.config'
            }
        }
    }
    post {
        always { 
            slackSend (channel: '#jenkins_gitleaks', color: '#0000FF', message: "Starting check for leaks - ${env.JOB_NAME} (<${env.BUILD_URL}|Open>)")
        }
        success {
            slackSend (channel: '#jenkins_gitleaks', color: '#00FF00', message: "SUCCESSFUL: Gitleaks didn't find any secret leaks $repo repo")
    }
        failure {
            slackSend (channel: '#jenkins_gitleaks', color: '#FF0000', message: "${custom_msg()}" )
      }
   }
}
def custom_msg()
{
  def JENKINS_URL= "${env.BUILD_URL}"
  def JOB_NAME = env.JOB_NAME
  def BUILD_ID= env.BUILD_ID
  def FAIL_REASON= "There are some passwords or secrets in $repo repo"
  def JENKINS_LOG= " FAILED: Job [${env.JOB_NAME}] Reason: ${FAIL_REASON} "
  return JENKINS_LOG
}