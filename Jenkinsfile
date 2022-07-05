pipeline {
  agent any

  stages {
    stage('Example') {
      steps {
        echo "hello"
        // sh "git diff upstream/master HEAD~"
        script {
          def target_branch = env.CHANGE_TARGET;
          def pr_ref        = env.BRANCH_NAME;
          echo sh(returnStdout: true, script: 'env')
          sh "git fetch --no-tags --progress -- ${env.GIT_URL} +refs/heads/main:refs/remotes/upstream/main"
          //sh "git fetch -- ${env.GIT_URL} origin master:refs/remotes/origin/master"
          sh "git diff HEAD~ origin/main"
          def TARGET = sh(returnStdout: true, script: "git rev-parse HEAD").trim()
          def HEAD   = sh(returnStdout: true, script: "git rev-parse origin/${pr_ref}").trim()
          echo "Checking for source changes between ${TARGET} (${HEAD}) and ${HEAD} (${pr_ref})...";
        }
      }
    }
  }
}
