@Library('lexmarkweb-jenkins-library') _

pipeline {
  agent { label 'linux'}
  parameters {
    string(defaultValue: "all", description: 'Which core(s)?', name: 'CORES')
    string(defaultValue: "vanilla", description: 'Name of favorite Ice cream flavor.', name: 'flavor')
  }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  stages {
    stage('init') {
      echo "You like ${params.flavor} Ice cream"
    }
    stage('build') {
      parallel {
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
    stage('final') {
      echo "You like ${flavor} Ice cream"
    }
  }
  post {
    success {
      echo "I did it."
      helloWorld name: "Messenger"
    }
  }
}
