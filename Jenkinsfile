pipeline {
  agent any
  stages {
    stage('Init') {
      steps {
        git(url: 'https://github.com/ThomasMosigFrey/jenkins.git', branch: 'master', poll: true)
      }
    }
    stage('Compile') {
      steps {
        dir(path: 'SimpleApp') {
          sh 'mvn compile'
        }
      }
    }
    stage('Package') {
      steps {
        dir(path: 'SimpleApp') {
          sh 'mvn package -DskipTests'
          archiveArtifacts(artifacts: 'target/*jar', allowEmptyArchive: true, caseSensitive: true)
        }
      }
    }
  }
  post {
      success {
       sh 'echo mvn deploy ...'
       emailext( to: 'thomas@mosig-frey.de' , subject : 'build successful' body: 'Dear user, ...' ) 
      }
  }
}
