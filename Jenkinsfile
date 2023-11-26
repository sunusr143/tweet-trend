   
   
   pipeline {
    agent any

    environment {
    PATH = "/opt/apache-maven/bin:$PATH"
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


    stage(" Docker Build ") {
      steps {
        script {
             def imageName = 'sunu.jfrog.io/sunu-docker/trend'
             def version   = '2.1.2'
               def registry = 'https://sunu.jfrog.io'
           echo '<--------------- Docker Build Started --------------->'
           app = docker.build(imageName+":"+version)
           echo '<--------------- Docker Build Ends --------------->'
        }
      }
    }

            stage (" Docker Publish "){
        steps {
            script {
                def registry = 'https://sunu.jfrog.io'
               echo '<--------------- Docker Publish Started --------------->'  
                docker.withRegistry(registry, 'jenkins-jfrog'){
                    app.push()
                }    
               echo '<--------------- Docker Publish Ended --------------->'  
            }
        }
    }

stage (" Deployto kube"){
  steps{
    script{
      sh './deploy.sh'
    }
  }
}

 }
   }
   