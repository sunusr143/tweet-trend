   
   
   pipeline {
    agent any

    environment {
    PATH = "/opt/apache-maven/bin:$PATH"
}
 stages{
       stage ("Git SCM"){
         steps{
             script{
                 git branch: 'main', url: 'https://github.com/sunusr143/tweet-trend.git'
             }
         }
     }
  
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

   
  //  stage('SonarQube analysis') {
  //   environment {
  //     scannerHome = tool 'sonar-scanner-devops'
  //   }
  //   steps{
  //   withSonarQubeEnv('sonarforjenkins') { // If you have configured more than one global server connection, you can specify its name
  //     sh "${scannerHome}/bin/sonar-scanner"
  //   }
  //   }
  // }

//     stage("Quality Gate"){
//     steps {
//         script {
//         timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
//     def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
//     if (qg.status != 'OK') {
//       error "Pipeline aborted due to quality gate failure: ${qg.status}"
//     }
//   }
// }
//     }
//   }
      
        //      stage("Jar Publish") {
        //     steps {
        //         script {
        //              def registry = 'https://sunu.jfrog.io/'
        //                 echo '<--------------- Jar Publish Started --------------->'
        //                  def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"jfrog jenkins"
        //                  def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
        //                  def uploadSpec = """{
        //                       "files": [
        //                         {
        //                           "pattern": "jarstaging/(*)",
        //                           "target": "libs-release-local/{1}",
        //                           "flat": "false",
        //                           "props" : "${properties}",
        //                           "exclusions": [ "*.sha1", "*.md5"]
        //                         }
        //                      ]
        //                  }"""
        //                  def buildInfo = server.upload(uploadSpec)
        //                  buildInfo.env.collect()
        //                  server.publishBuildInfo(buildInfo)
        //                  echo '<--------------- Jar Publish Ended --------------->'  
                
        //         }
        //     }   
        // }
   def imageName = 'sunu.jfrog.io/sunu-docker/trend'
   def version   = '2.1.2'
    stage(" Docker Build ") {
      steps {
        script {
           echo '<--------------- Docker Build Started --------------->'
           app = docker.build(imageName+":"+version)
           echo '<--------------- Docker Build Ends --------------->'
        }
      }
    }

            stage (" Docker Publish "){
        steps {
            script {
               echo '<--------------- Docker Publish Started --------------->'  
                docker.withRegistry(registry, 'artifactory_token'){
                    app.push()
                }    
               echo '<--------------- Docker Publish Ended --------------->'  
            }
        }
    }



 }
   }
   