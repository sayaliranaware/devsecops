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
          sh 'scp -o StrictHostkeyChecking=no target/*.war ubuntu@18.190.67.222:/home/ubuntu/prod/apache-tomcat-9.0.65/webapps/webapp.war'
          }
      }
    }
    //stage ('Check-Git-Secrets') { 
      //steps {
        //sh 'docker pull gesellix/trufflehog' 
        //sh 'docker run -t gesellix/trufflehog --json https://github.com/devopssecure/webapp.git › trufflehog'
          //}
    //}
  }
}  
