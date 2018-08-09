@Library('lexmarkweb-jenkins-library') _

pipeline {
  agent { label 'linux'}
  stages {
    stage('adventure') {
      steps {
        echo "The branch name is $BRANCH_NAME"
        helloWorld name:'Penelope'
      }
    }
    stage('build') {
      when {
        anyOf {
          branch 'release-*'
          branch 'dev'          
        }
      }
      steps {
        echo "This is a dev or release build."
      }
    }
  }
}
