node
{

    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([githubPush()])])
    def MAVEN_HOME = tool name: "Maven-3.8.6"
    
    stage('GitSCM')
    {
        git branch: 'development', credentialsId: 'f3c752dc-146b-4b98-800e-78f206f0abaa', url: 'https://github.com/swapnildhere/maven-web-application.git'
    }
    
    stage('Build')
    {
       sh "${MAVEN_HOME}/bin/mvn clean package"
    }
    stage('SonarQubeScanner')
    {
        sh "${MAVEN_HOME}/bin/mvn sonar:sonar"
    }
    stage('NexusServer')
    {
        sh "${MAVEN_HOME}/bin/mvn deploy"
    }
    stage('ApacheTomcat')
    {
       deploy adapters: [tomcat9(credentialsId: 'aa66480a-6032-4ff6-a3db-2dd5e40ad41a', path: '', url: 'http://34.201.126.65:8080/')], contextPath: '/swappy', war: '**/maven-web-application.war'
    }
    
}
