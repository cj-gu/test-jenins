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
          echo "${env.CHANGE_TARGET}"
          def TARGET = sh(returnStdout: true, script: "git rev-parse upstream/${target_branch}").trim()
          def HEAD   = sh(returnStdout: true, script: "git rev-parse origin/${pr_ref}").trim()
          echo "Checking for source changes between ${TARGET} (${target_branch}) and ${HEAD} (${pr_ref})...";
        }
      }
    }
  }
}
