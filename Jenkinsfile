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
    stage('init') {
      steps {
        script {
          def cookie = "Chocolate"
        }
      }
    }
    stage('build') {
      parallel {
        stage('build-prod') {
          when {
            branch 'master'
          }
          steps {
            echo "build ${params.CORES} to ${cookie} Production"
          }
        }
        stage('build-qa') {
          when {
            branch 'master'
          }
          steps {
            echo "build ${params.CORES} to ${cookie} QA"
          }
        }
        stage('build-dev') {
          when {
            branch 'dev'
          }
          steps {
            echo "build ${params.CORES} to ${cookie} Dev"
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
