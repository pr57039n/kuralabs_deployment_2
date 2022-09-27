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
        py.test --verbose --junit-xml test-reports/results.xml
        ''' 
      }
    
      post{
        always {
          junit 'test-reports/results.xml'
        }
       
      }
    }
   stage ('Deploy') {
      steps {
            sh '/var/lib/jenkins/.local/bin/eb deploy Deployment-2-main-dev
      }
    }
        stage('Notify') {
            steps {
              echo "Done"
            }
    post {
        always {
            emailext body: 'If you get this message the application deployed and this worked.', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Successful build'
        }
    }
  }
   }
}
