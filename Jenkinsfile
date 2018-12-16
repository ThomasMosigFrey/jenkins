pipeline {
  agent any
  
  stages {
    stage('init') {
      steps {
        git(url: 'https://github.com/ThomasMosigFrey/jenkins.git', branch: 'master', poll: true)
        echo 'start'
        dir(path: 'SimpleApp') {
          sh 'echo Init'
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
