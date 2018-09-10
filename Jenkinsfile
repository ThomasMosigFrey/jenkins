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
  }
}