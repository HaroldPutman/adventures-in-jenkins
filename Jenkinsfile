@Library('lexmarkweb-jenkins-library') _

pipeline {
  agent { label 'linux'}
  parameters {
    string(defaultValue: "all", description: 'Which core(s)?', name: 'CORES')
  }
  stages {
    stage('build-prod') {
      when {
        branch 'master'
      }
      steps {
        echo "build ${params.CORES} to Production"
      }
    }
    stage('build-qa') {
      when {
        branch 'release-*'
      }
      steps {
        echo "build ${params.CORES} to QA"
      }
    }
    stage('build-dev') {
      when {
        branch 'dev'
      }
      steps {
        echo "build ${params.CORES} to Dev"
      }
    }
  }
  post {
    always {
      echo "I did it in Release."
      helloWorld name: "Wally"
    }
  }
}
