@Library('lexmarkweb-jenkins-library') _

pipeline {
  agent { label 'linux'}
  parameters {
    string(defaultValue: "all", description: 'Which core(s)?', name: 'CORES')
  }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  stages {
    stage('build') {
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
            branch 'master'
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
    }
    stage('wrapup') {
      steps {
        script {
          currentBuild.description = "Built ${params.CORES}"
        }
      }
    }
  }
  post {
    always {
      echo "I did it."
      helloWorld name: "Sally"
    }
  }
}
