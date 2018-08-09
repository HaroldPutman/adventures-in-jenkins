@Library('lexmarkweb-jenkins-library') _

pipeline {
  agent { label 'linux'}
  parameters {
    string(defaultValue: "all", description: 'Which core(s)?', name: 'CORES')
  }
  stages {
    stage('adventure') {
      steps {
        echo "The branch name is $BRANCH_NAME"
        echo params.CORES
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
        nodejs(nodeJSInstallationName: 'node 8 latest') {
          sh 'node --version'
          sh 'npm --version'
        }
      }
    }
  }
}
