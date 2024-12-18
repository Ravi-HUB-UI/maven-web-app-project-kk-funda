pipeline {
    agent any 
tools
{
  maven "Maven 3.9.9"
}
    parameters {
  choice choices: ['main', 'development'], name: 'Branches'
}
/*triggers {
  pollSCM '* * * * *'
}*/
options {
  timestamps()
}
    stages {
        stage('checkout stage') { 
            steps {
              notifyBuild('STARTED')
            git branch: 'main', credentialsId: '82419a66-ac17-4150-b55e-076e0d0095a8', url: 'https://github.com/Ravi-HUB-UI/maven-web-app-project-kk-funda.git'
                
            }
        }
            stage('Build') { 
            steps {
         sh "mvn clean package"
                
            }
          }
          stage('SQREPORT'){
            steps{
          sh "mvn sonar:sonar"
          }
        }
         stage('DeployTONexus'){
           steps{
          sh "mvn deploy"
          }
        }
        stage('DeployAppToTomcat'){
           steps{
         sshagent(['e200fa8c-8a2e-4662-8e97-0e827ca8653b']) 
      {
   
      sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.203.160.188:/opt/apache-tomcat-9.0.97/webapps/"
    
      }

         
          }
        }
    } //stages closing
post {
  success {
    notifyBuild(currentBuild.result)
  }
  failure {
notifyBuild(currentBuild.result)
      }
}
}//pipeline closing


def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}
