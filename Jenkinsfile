node
{
   def mavenHome=tool name: "Maven 3.9.9"
   parameters {
  choice choices: ['main', 'development'], name: 'Branches'
}
   stage('checkout')
   {
   git branch: '$Branches', credentialsId: '82419a66-ac17-4150-b55e-076e0d0095a8', url: 'https://github.com/Ravi-HUB-UI/maven-web-app-project-kk-funda.git'
   }
   stage('build')
   {
  sh "${mavenHome}/bin/mvn clean package"
   }
  stage('SonarQubeReport')
   {
  sh "${mavenHome}/bin/mvn clean package sonar:sonar"
   }

 stage('Uploadarifacttonexus')
   {
  sh "${mavenHome}/bin/mvn clean package sonar:sonar deploy"
   }

   stage('deployapptotomcat')
   {
     sshagent(['e200fa8c-8a2e-4662-8e97-0e827ca8653b'])
    {
    
     sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.111.36.130:/opt/apache-tomcat-9.0.97/webapps/"

     }
   }  
}

