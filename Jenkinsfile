pipeline {
  agent {
    docker {
      image 'maven:3.6.3-jdk-11'
    }
  }

  options {
    buildDiscarder(logRotator(numToKeepStr: '30', artifactNumToKeepStr: '3'))
  }

  triggers {
    cron '@midnight'
  }

  stages {    
    stage('build') {
      steps {
        script {
          def phase = isReleaseOrMasterBranch() ? 'deploy' : 'verify'
          maven cmd: "clean ${phase} -Dmaven.test.skip=true"
        }
        archiveArtifacts 'target/*.jar'
      }
    }
  }
}

def isReleaseOrMasterBranch() {
  return env.BRANCH_NAME == 'master' || env.BRANCH_NAME.startsWith('release/') 
}
