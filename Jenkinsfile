pipeline {
  tools {
    maven 'Maven'
  }
  agent any
  stages  {
    stage ('Initialize')  {
      steps {
        sh  ''' 
                     echo "PATH = $(PATH)"
                     echo "M2_HOME = $(M2_HOME)"
             '''
      }
    }
    
    stage ('Bulld') {
      steps {
      sh 'mvn clean package'
      }
    }
    
    stage ('Deploy-To-Tomcat') {
      steps {
        sshagent(['tomcat']) {
          //sh 'scp -o StrictHostkeyChecking=no target/*.jar ubuntu@3.130.38.171:/home/ubuntu/prod/apache-tomcat-9.0.65/webapps/webapp.jar'
          sh 'scp -o StrictHostkeyChecking=no target/*.war ubuntu@3.130.38.171:/home/ubuntu/prod/apache-tomcat-9.0.65/webapps/webapp.war'
          }
      }
    }
  
  }
}  
