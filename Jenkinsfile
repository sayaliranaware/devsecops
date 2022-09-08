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
        sh 'wget "https://raw.githubusercontent.com/sayaliranaware/webapp/master/Owasp-Dependency-Check.sh" '
        sh 'chmod +x Owasp-Dependency-Check.sh'
        sh 'bash Owasp-Dependency-Check.sh'
        sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
        
      }
    }
    stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
          sh 'cat target/sonar/report-task.txt'
        }
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
    stage ('DAST') {
      steps {
        sshagent(['zap']) {
         sh 'ssh -o  StrictHostKeyChecking=no ubuntu@3.140.15.163 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://18.190.67.222:8080/webapp/" || true'
        }
      }
    }
  }
}  
