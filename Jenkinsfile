pipeline {
  agent any

  stages {
    stage('Example') {
      steps {
        echo "hello"
        sh "git diff +refs/heads/master:refs/remotes/upstream/master HEAD~"
      }
    }
  }
}
