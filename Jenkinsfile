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
  }
}
