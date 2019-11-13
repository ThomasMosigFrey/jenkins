import de.dhl.jenkins.groovy.*;

pipeline {
  agent none

  libraries {
        lib 'DHL_Jenkins_LIB@1.0'
  }

  tools {
    jdk 'linux_jdk1.8.0_172'
    maven 'linux_M3'
  }

  stages {
    stage('parallel compiles') {
      parallel {
        stage('SimpleApp') {
          agent { label 'linux'}
          steps {
            lock(resource: 'Lock') {
              HelloWorld.sayHello();
              dir(path: 'SimpleApp') {
                sh '${MAVEN_HOME}/bin/mvn -s ../maven/settings.xml clean compile'
              }
            }
          }  
        }
        stage('stateless') {
          agent { label 'linux'}
          steps {
            lock(resource: 'Lock') {
              dir(path: 'ejb/stateless') {
                sh '${MAVEN_HOME}/bin/mvn -s ../../maven/settings.xml clean compile'
              }
            }
          }
        }
        stage('stateful') {
          agent { label 'linux'}
          steps {
            lock(resource: 'Lock') {
              dir(path: 'ejb/stateful') {
                sh '${MAVEN_HOME}/bin/mvn -s ../../maven/settings.xml clean compile'
              }
            }
          } 
        }
      stage('jms') {
          agent { label 'linux'}
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
      agent { label 'linux'}
      steps {
        dir(path: 'SimpleApp') {
          sh '${MAVEN_HOME}/bin/mvn test'
        }
        junit '**/surefire*/*.xml'
      }
    }
    stage('save_check_results') {
      agent { label 'linux'}
      steps {
        archiveArtifacts artifacts: '**/*surefire*/*.xml' , allowEmptyArchive: true
      }
    } 

    stage('package') {
      agent { label 'linux'}
      steps {
        dir(path: 'SimpleApp') {
          sh '${MAVEN_HOME}/bin/mvn package'
        }      
      }
    }

    stage('upload_artifacts') {
      agent { label 'linux'}
      steps {
        archiveArtifacts artifacts: '**/*.jar',  allowEmptyArchive: true
      }
    }
  }
 
  post {
    failure {
        echo 'I will always say Hello again!'
    }
  }
}
