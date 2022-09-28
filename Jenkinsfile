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
        
      } 
       
       post{
         success {
            slackSend(message: """DEPLOYMENT SUCCESSFUL
            ${custom_msg()}""")
        }
         failure {
            slackSend(message: """DEPLOYMENT FAILED
            ${custom_msg()}""")
        }
       }
       
     }    
        
  }
 }


def custom_msg()
{
  def JENKINS_LOG= """Job: [${env.JOB_NAME}]
  Path to log of each step: ${env.BUILD_URL}consoleText"""
  return JENKINS_LOG
}

