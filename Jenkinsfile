pipeline {
  agent any
  stages {
    stage('init') {
      steps {
        git(url: 'https://thomasfrey@bitbucket.org/thomasfrey/jenkins_repository.git', branch: 'master', poll: true)
        echo 'start'
        dir(path: 'SimpleApp') {
          sh 'echo Init'
        }

      }
    }
    stage('Test') {
      steps {
        tool(name: 'MAVEN', type: 'maven')
        dir(path: 'SimpleApp') {
          sh 'mvn test'
          archiveArtifacts(artifacts: '**/junit/*xml', allowEmptyArchive: true, caseSensitive: true)
        }

      }
    }
    stage('Package') {
      steps {
        dir(path: 'SimpleApp') {
          sh 'mvn package -DskipTests'
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