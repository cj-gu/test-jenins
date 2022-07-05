pipeline {
  agent any

  stages {
    stage('Example') {
      steps {
        echo "hello"
        sh "git diff master HEAD~"
      }
    }
  }
}
