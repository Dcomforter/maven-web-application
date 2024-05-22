pipeline{
    
  agent any 
  
  tools {
    maven "Maven3"
  }  
  
  stages {
    stage('1GetCode'){
      steps{
        sh "echo 'cloning the latest application version' "
        git url: "https://github.com/Dcomforter/maven-web-application.git"
      }
    }
    
    stage('2Test+Build'){
      steps{
        sh "echo 'running JUnit-test-cases' "
        sh "echo 'Tests must be passed to create artifacts ' "
        sh "mvn clean package"
      }
    }
    
    stage('3CodeQuality'){
      steps{
        sh "echo 'Perfoming CodeQualityAnalysis on SonarQube' "
        sh "mvn sonar:sonar"
      }
    }
   
    stage('4uploadNexus'){
      steps{
        sh "echo 'Deploying artifacts to Nexus' "
        sh "mvn deploy"
      }
    } 
    
    stage('5deploy2prod'){
      steps{
        deploy adapters: [tomcat9(credentialsId: 'tomcatcred', path: '', url: 'http://24.144.95.173:8177/')], contextPath: null, war: 'target/*war'
      }
    }
  }
  
  post{
    always{
      emailext body: '''Hey guys
      Please check build status.

      Thanks
      
      Landmark 
      +1 437 215 2483''', recipientProviders: [buildUser(), developers()], subject: 'success', to: 'israelokuneye@gmail.com'
    }
    
    success{
      emailext body: '''Hey guys,

      Good job, build and deployment is successful.

      Best regards,''', recipientProviders: [buildUser(), developers()], subject: 'Application Deployed to Tomcat', to: 'israelokuneye@gmail.com'
    } 
  
    failure{
      emailext body: '''Hey guys,

      Good job, build and deployment is successful.

      Best regards,''', recipientProviders: [buildUser(), developers()], subject: 'Application Deployed to Tomcat', to: 'israelokuneye@gmail.com'
    }
  }
 }
