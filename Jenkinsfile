   
   
   pipeline {
    agent any

    environment {
    PATH = "/opt/apache-maven-3.9.2/bin:$PATH"
}
 stages{
    stage("build"){
            steps {
                 echo "----------- build started ----------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                 echo "----------- build complted ----------"
            }
        }
        stage("test"){
            steps{
                echo "----------- unit test started ----------"
                sh 'mvn surefire-report:report'
                 echo "----------- unit test Complted ----------"
            }
        }
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
    withSonarQubeEnv('sonarforjenkins') { // If you have configured more than one global server connection, you can specify its name
      sh "${scannerHome}/bin/sonar-scanner"
    }
    }
  }
 }
   }
   