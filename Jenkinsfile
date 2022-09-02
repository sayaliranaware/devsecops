pipeline {
  tools {
    maven 'Maven'
  }
  agent any
  stages  {
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
    stage ('Check-Git-Secrets') { 
      steps {
        //sh 'docker pull gesellix/trufflehog' 
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/sayaliranaware/webapp.git > trufflehog'
        sh 'cat trufflehog'
          }
    }
    
    stage ('Source Composition Analysis'){
      steps{
        sh 'rm Owasp* || true'
        sh 'wget 'https://raw.githubusercontent.com/sayaliranaware/webapp/master/Owasp-Dependency-Check.sh''
        sh 'shmod +x Owasp-Dependency-Check.sh'
        sh 'bash Owasp-Dependency-Check.sh'
        
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
    
  }
}  
