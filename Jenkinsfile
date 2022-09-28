pipeline {
  agent any
   stages {
     
     stage ('build') {
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
        echo "Finished deployment of Build #${env.BUILD_ID} on ${env.JENKINS_URL}"
      } 
       
     }    
     
   
     
     
   
  }
 }
