pipeline { 
    environment {
    user = "turik207"
    repo = "petclinic-db"
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
            // slackSend message: "${custom_msg()}"
            slackSend message: "succ"
    }
        failure {
            slackSend message: "fail"
            // slackSend message: "${custom_msg()}"
            // slackSend message: "Build failed - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
      }
   }
}
// def custom_msg()
// {
//   def JENKINS_URL= "${env.BUILD_URL}"
//   def JOB_NAME = env.JOB_NAME
//   def BUILD_ID= env.BUILD_ID
//   def FAIL_REASON= "tapildi"
//   def JENKINS_LOG= " FAILED: Job [${env.JOB_NAME}] Reason:${FAIL_REASON} Logs path: ${JENKINS_URL}/job/${JOB_NAME}/${BUILD_ID}/consoleText "
//   return JENKINS_LOG
// }

def directory = "${env.WORKSPACE}/${env.JOB_NAME}" // change name here
stage('capture console output') {
  script {
    def logContent = Jenkins.getInstance().getItemByFullName(env.JOB_NAME).getBuildByNumber(
    Integer.parseInt(env.BUILD_NUMBER)).logFile.text
    // copy the log in the job's own workspace
    writeFile file: directory + "/fail-output",
    text: logContent
  }
  def consoleOutput = readFile directory + '/fail-output'
  echo 'Console output saved in the fail-output file'
  echo '--------------------------------------'
  echo consoleOutput
  echo '--------------------------------------'
}