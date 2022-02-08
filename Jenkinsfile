node{
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    def mavenHome= tool name: "maven3.8.4"
    //Get the code from github repo
    stage('checkout'){
     git branch: 'development', credentialsId: 'f8b303fd-bc5a-4469-ad27-8e6948b2fc8c', url: 'https://github.com/nandu-vyas19/maven-web-application.git'
 }
 //using maven doing the build
stage('build'){
    sh "${mavenHome}/bin/mvn clean package"
}
//executing the sonarqube report
 stage('executesonarqubereport'){
 sh "${mavenHome}/bin/mvn sonar:sonar"     
 }
 //uploading Artifacts into nexus repo
  stage('uploadArtifactintoNexusRepo'){
  sh "${mavenHome}/bin/mvn deploy"      
  }
  //deploying application into tomcat
  stage('DeployAppintoTomcatserver'){
    sshagent(['c889f7d6-b361-4045-ba5b-951834d022c9']){
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.235.115.123:/opt/apache-tomcat-9.0.58/webapps/"        
    } 
        
  }
}//Node close
