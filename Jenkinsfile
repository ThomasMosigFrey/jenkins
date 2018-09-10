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
        sh 'mvn test'
      }
    }
    stage('Package') {
      steps {
        sh 'mvn package'
      }
    }
    stage('Install') {
      steps {
        sh 'mvn install'
      }
    }
    stage('deploy') {
      steps {
        sh 'mvn deploy'
      }
    }
  }
}