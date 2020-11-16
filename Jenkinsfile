pipeline {
  agent {
    kubernetes {
      //cloud 'kubernetes'
      yaml """
kind: Pod
metadata:
  name: kaniko
spec:
  containers:
  - name: katalon
    image: katalonstudio/katalon
    imagePullPolicy: Always
    command:
    - cat
    tty: true
"""
    }
  }
  stages {
    stage('Build') {
      steps {
        container('katalon') {
            sh '''           
               katalonc -noSplash -runMode=console -projectPath="/home/jenkins/agent/workspace/katalon/test.prj" -retry=0 -testSuitePath="Test Suites/TS_RegressionTest" -executionProfile="default" -browserType="Chrome (headless)" -apiKey="322794a9-309d-42d4-a9b2-ff2a82308828" --config -proxy.auth.option=NO_PROXY -proxy.system.option=NO_PROXY -proxy.system.applyToDesiredCapabilities=true
               '''
        } 
      }    
    }
  }
  post {
        always {
            archiveArtifacts artifacts:  'Reports/*/TS_RegressionTest/**/*.*', fingerprint: true
            junit 'Reports/*/TS_RegressionTest/**/JUnit_Report.xml'
           
        }
     }
  }
