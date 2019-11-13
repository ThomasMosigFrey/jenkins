
Skip to content
Pull requests
Issues
Marketplace
Explore
@ThomasMosigFrey

1
0

    0

ThomasMosigFrey/DHL
Code
Issues 0
Pull requests 0
Projects 0
Wiki
Security
Insights
Settings
DHL/Jenkinsfile
@ThomasMosigFrey ThomasMosigFrey Update Jenkinsfile 558f311 11 minutes ago
47 lines (44 sloc) 903 Bytes
pipeline {
  agent { label 'linux' }
  tools {
    jdk 'linux_jdk1.8.0_172'
    maven 'linux_M3'
  }
  stages {
    stage('compile') {
      steps {
        dir(path: 'SimpleApp') {
          sh '${MAVEN_HOME}/bin/mvn -X -s maven/settings.xml clean compile'
        }
      }
    }
    stage('sonarqube_checks') {
      steps {
        dir(path: 'SimpleApp') {
          sh '${MAVEN_HOME}/bin/mvn sonar:sonar'
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
        archiveArtifacts artifacts: '**/surefire*/*.xml' , allowEmptyArchive: true
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
