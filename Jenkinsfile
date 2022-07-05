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
          sh "git fetch origin master:refs/remotes/origin/master"
          sh "git diff HEAD~ upstream/master"
          def TARGET = sh(returnStdout: true, script: "git rev-parse HEAD").trim()
          def HEAD   = sh(returnStdout: true, script: "git rev-parse origin/${pr_ref}").trim()
          echo "Checking for source changes between ${TARGET} (${HEAD}) and ${HEAD} (${pr_ref})...";
        }
      }
    }
  }
}
