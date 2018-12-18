pipeline {
  agent any
  stages {
    stage('Init') {
      agent {
        node {
          label 'unix'
        }

      }
      steps {
        git(url: 'https://github.com/ThomasMosigFrey/jenkins.git', branch: 'master', poll: true)
        timeout(time: 4, unit: 'MINUTES') {
          dir(path: 'SimpleApp') {
            sh 'echo Init'
            sh 'mvn compile'
          }

          dir(path: 'scripts') {
            sh 'sh ./runTests.sh'
          }

        }

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
}