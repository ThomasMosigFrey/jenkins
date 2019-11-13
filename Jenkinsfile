pipeline {
  agent {
    node {
      label 'linux'
    }

  }
  stages {
    stage('parallel compiles') {
      parallel {
        stage('SimpleApp') {
          agent {
            label 'linux'
          }
          steps {
            lock(resource: 'Lock') {
              dir(path: 'SimpleApp') {
                sh '${MAVEN_HOME}/bin/mvn -s ../maven/settings.xml clean compile'
              }

            }

          }
        }

        stage('stateless') {
          agent {
            label 'linux'
          }
          steps {
            lock(resource: 'Lock') {
              dir(path: 'ejb/stateless') {
                sh '${MAVEN_HOME}/bin/mvn -s ../../maven/settings.xml clean compile'
              }

            }

          }
        }

        stage('stateful') {
          agent {
            label 'linux'
          }
          steps {
            lock(resource: 'Lock') {
              dir(path: 'ejb/stateful') {
                sh '${MAVEN_HOME}/bin/mvn -s ../../maven/settings.xml clean compile'
              }

            }

          }
        }

        stage('jms') {
          agent {
            label 'linux'
          }
          steps {
            lock(resource: 'Lock') {
              dir(path: 'jms') {
                sh '${MAVEN_HOME}/bin/mvn -s ../maven/settings.xml clean compile'
              }

            }

          }
        }

      }
    }

    stage('test') {
      agent {
        label 'linux'
      }
      steps {
        dir(path: 'SimpleApp') {
          sh '${MAVEN_HOME}/bin/mvn test'
        }

        junit '**/surefire*/*.xml'
      }
    }

    stage('save_check_results') {
      agent {
        label 'linux'
      }
      steps {
        archiveArtifacts(artifacts: '**/*surefire*/*.xml', allowEmptyArchive: true)
      }
    }

    stage('package') {
      agent {
        label 'linux'
      }
      steps {
        dir(path: 'SimpleApp') {
          sh '${MAVEN_HOME}/bin/mvn package'
        }

      }
    }

    stage('upload_artifacts') {
      agent {
        label 'linux'
      }
      steps {
        archiveArtifacts(artifacts: '**/*.jar', allowEmptyArchive: true)
      }
    }

  }
  tools {
    jdk 'linux_jdk1.8.0_172'
    maven 'linux_M3'
  }
  post {
    failure {
      echo 'I will always say Hello again!'
    }

  }
}