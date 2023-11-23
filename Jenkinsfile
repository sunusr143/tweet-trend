   
   
   pipeline {
    agent any
 stages{
     stage ("Git SCM"){
         steps{
             script{
                 git branch: 'main', url: 'https://github.com/sunusr143/tweet-trend.git'
             }
         }
     }
  
   
   stage('SonarQube analysis') {
    environment {
      scannerHome = tool 'sonar-scanner-devops'
    }
    steps{
    withSonarQubeEnv('sonar-scanner-devopsr') { // If you have configured more than one global server connection, you can specify its name
      sh "${scannerHome}/bin/sonar-scanner"
    }
    }
  }
 }
   }
   