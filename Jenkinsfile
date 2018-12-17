pipeline {
  agent any
  stages {
    stage('Init') {
      parallel {
        stage('Init unix') {
          steps {
            git(url: 'https://github.com/ThomasMosigFrey/jenkins.git', branch: 'master', poll: true)
            timeout(time: 4, unit: 'MINUTES') {
              dir(path: 'SimpleApp') {
                sh 'echo Init'
                sh 'mvn compile'
              }

              dir(path: 'scripts') {
                sh 'sh runTests.sh'
              }

            }

          }
        }
        stage('init windows') {
          steps {
            git(url: 'https://github.com/ThomasMosigFrey/jenkins.git', branch: 'master')
          }
        }
      }
    }
    stage('Test') {
      steps {
        dir(path: 'SimpleApp') {
          sh 'mvn test'
          archiveArtifacts(artifacts: 'target/surefire-reports/*xml', allowEmptyArchive: true, caseSensitive: true)
          junit(testResults: 'target/surefire-reports/*xml', allowEmptyResults: true)
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
    stage('Install') {
      steps {
        dir(path: 'SimpleApp') {
          sh 'mvn install -DskipTests'
        }

      }
    }
  }
}