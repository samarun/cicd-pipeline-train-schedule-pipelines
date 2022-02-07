pipeline {
 agent any
  stages {
    stage ('Build') {
      steps {
       echo 'Running build automation'
       sh './gradlew build --no-daemon'
       archiveArtifacts artifacts: 'dist/trainSchedule.zip'
      }
    }
   stage('DeployToStaging') {
    when {
     branch 'master'
    }
    steps {
     whithCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS']) {
      sshPublisher(
       failOnError: true,
       continueOnError: false;
       publisher: [
        sshPublisherDesc(
         configName: 'staging',
          sshCredentials: [
           username:"USERNAME",
           encryptionPassphrase: "USERPASS"
          ],
          transfers: [
           sshTransfer(
            sourceFiles: 'dist/trainSchedule.zip',
            removePrefix: 'dist/',
            remoteDirectory: '/tmp',
            execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/tainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
            
           )
          ]
         
         
        )
        
       ]
       
      )
      
     }
    }
   }
  }
}
