
def find_py = { base ->
    return sh(returnStdout: true, script: "git diff --name-only HEAD ${base} | grep ^py/ | sed -n '\$='").trim()
}

pipeline {
  agent any
  options {
    skipDefaultCheckout()

  }

  stages {
    stage('Checkout') {
      steps {
        // checkout scm
        echo "AAAAAA"
        checkoutWithTags()
      }
    }
    stage('Example') {
      when {
        expression {
          //return shouldRunFETest(find_py)
          pattern = "'^frontend/packages/\\|^py/'"
          shouldRunTest = sh(returnStdout: true, script: "git diff --name-only HEAD main4 | grep ${pattern} | sed -n '\$='").trim()
          echo "shouldRunTest=${shouldRunTest}"
          return false
        }
      }

      steps {
        echo "hello"
        // sh "git diff upstream/master HEAD~"
        script {
          def target_branch = env.CHANGE_TARGET;
          def pr_ref        = env.BRANCH_NAME;
          //echo "params.BRANCH=${params.BRANCH}"
          //echo sh(returnStdout: true, script: 'params')
          echo "======================="
          echo sh(returnStdout: true, script: 'env')
          //sh 'git fetch --no-tags --progress -- "https://github.com/cj-gu/test-jenins.git" +refs/heads/main:refs/remotes/upstream/main'
          //sh "git fetch -- ${env.GIT_URL} origin master:refs/remotes/origin/master"
          //sh "git diff --name-only HEAD~ upstream/main | grep ^py/ | sed -n '\$='"
          echo "git branch -r"
          sh "git branch -r"
          echo "git branch"
          sh "git branch"
          //sh "git diff --name-only HEAD~ upstream/main | grep ^py/ | sed -n '\$='"
          sh "git diff --name-only HEAD main4"
          def TARGET = sh(returnStdout: true, script: "git rev-parse HEAD").trim()
          def HEAD   = sh(returnStdout: true, script: "git rev-parse origin/${pr_ref}").trim()
          echo "Checking for source changes between ${TARGET} (${HEAD}) and ${HEAD} (${pr_ref})...";
          sh "AA=${env.CHANGE_ID} echp ${AA}"
        }
      }
    }
  }
}

def checkoutWithTags() {
    echo "${scm.branches[0].getName()}"
    echo "${scm.userRemoteConfigs[0].getName()}"
    echo "${scm.userRemoteConfigs[0].getUrl()}"
    echo "${scm.userRemoteConfigs[0].getRefspec()}"
    
    /*checkout([
      $class: 'GitSCM',
      branches: [[name: 'main']],
      doGenerateSubmoduleConfigurations: false,
      extensions: scm.extensions + [[$class: 'LocalBranch']],
      doGenerateSubmoduleConfigurations: scm.doGenerateSubmoduleConfigurations,
      userRemoteConfigs: scm.userRemoteConfigs,
    ])*/
    //scm.verbose = true
    //scm.userRemoteConfigs[0].refspec = "+refs/heads/main:refs/remotes/upstream/main"
    /*checkout([
        $class: 'GitSCM',
        branches: scm.branches + [[name: 'origin/main']],
        extensions: scm.extensions + [[
            $class: 'CloneOption',
            noTags: false,
            reference: '',
            shallow: true,

            // default depth is 1, which will make "git describe" fail to get right tag
            // so we add a rough buffer here
            // to make sure travel at most 300 commits depth, we can find the latest tag
            depth: 300,
        ] + [$class: 'LocalBranch', localBranch: "**"]],
        doGenerateSubmoduleConfigurations: scm.doGenerateSubmoduleConfigurations,
        userRemoteConfigs: scm.userRemoteConfigs,
        //scm.userRemoteConfigs,
    ])*/

    checkout([
      $class: 'GitSCM',
      branches: scm.branches,
      extensions: scm.extensions + [[
        $class: 'CloneOption',
        noTags: false,
        reference: '',
        shallow: true,

        // default depth is 1, which will make "git describe" fail to get right tag
        // so we add a rough buffer here
        // to make sure travel at most 300 commits depth, we can find the latest tag
        depth: 300,
      ]],
      userRemoteConfigs:
        scm.userRemoteConfigs +
        [
          [name: scm.userRemoteConfigs[0].getName(), url: scm.userRemoteConfigs[0].getUrl(), refspec: '+refs/heads/main:refs/remotes/main4']
        ]
    ])
    //[[url: 'https://github.com/cj-gu/test-jenins.git', refspec: "+refs/heads/main:refs/remotes/origin/main"]]]
}

def shouldRunFETest(Closure matcher) {
    echo "SCM_URL=${env.SCM_URL}"
    echo sh(returnStdout: true, script: 'env')
    // git plugin now defaults to not pulling all the git refs on clone, we have to fetch it manually.
    // sh "git fetch --no-tags --progress -- 'https://github.com/cj-gu/test-jenins.git' +refs/heads/fix-123:refs/remotes/origin/fix-123"
    // find the base commit id of this PR.
    base = sh(returnStdout: true, script: "git merge-base HEAD main").trim()
    echo "base=${base}"
    // Check whether any files in app folder are modified in this PR.
    diff = matcher(base)
    echo "diff=${diff}"

    return diff.length() > 0
}