

pipeline {
  agent none
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
            dir(path: 'SimpleApp') {
              sh '${MAVEN_HOME}/bin/mvn -X -s ../maven/settings.xml clean compile'
            }
          }  
        }
        stage('Complete') {
          agent { label 'linux'}
          steps {
            dir(path: 'ejb/stateless') {
              sh '${MAVEN_HOME}/bin/mvn -X -s ../../maven/settings.xml clean compile'
            }
          }  
        }
      }
    }
    stage('test') {
      steps {
        dir(path: 'SimpleApp') {
          sh '${MAVEN_HOME}/bin/mvn test'
        }
      }
    }
    stage('save_check_results') {
      steps {
        archiveArtifacts artifacts: '**/*surefire*/*.xml' , allowEmptyArchive: true
      }
    } 
    
    stage('package') {
      steps {
        dir(path: 'SimpleApp') {
          sh '${MAVEN_HOME}/bin/mvn package'
        }
      }
    }
    
    stage('upload_articats') {
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
