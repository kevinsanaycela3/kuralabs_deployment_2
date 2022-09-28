pipeline {
  agent any
   stages {
     
     stage ('Build') {
      steps {
        sh '''#!/bin/bash
        python3 -m venv test3
        source test3/bin/activate
        pip install pip --upgrade
        pip install -r requirements.txt
        export FLASK_APP=application
        flask run &
        '''
      }
       post{
         success {
            slackSend(message: "SUCCESS: ${custom_msg()}")
        }
         failure {
            slackSend(message: "FAILED: ${custom_msg()}")
        }
       }
     }
     
     stage ('test') {
      steps {
        sh '''#!/bin/bash
        source test3/bin/activate
        python3 -m py.test --verbose --junit-xml test-reports/results.xml
        ''' 
      }
       post {
        always {
          junit 'test-reports/results.xml'
        }
      }
    }
    
     stage ('deploy') {
      steps {
        sh '/var/lib/jenkins/.local/bin/eb deploy url-shortner-dev'
        echo "Finished deployment of Build #${env.BUILD_ID} on ${env.JENKINS_URL}"
      } 
       
     }    
        
  }
 }


def custom_msg()
{
  def JENKINS_URL= "http://54.210.254.27:8080"
  def JOB_NAME = env.JOB_NAME
  def BUILD_ID= env.BUILD_ID
  def JENKINS_LOG= " Job [${env.JOB_NAME}] Logs path: ${JENKINS_URL}/job/${env}/indexing/consoleText"
  return JENKINS_LOG
}

