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
      steps {
        echo "If we succeed, you get ${params.flavor} Ice cream"
      }
    }
    stage('build') {
      parallel {
        stage('build-prod') {
          when {
            branch 'master'
          }
          steps {
            echo "build ${params.CORES} to Production"
            dir ('my-app') {
              withMaven (maven: 'Maven Latest', jdk: 'JDK 1.8 Latest (Linux 64-bit)') {
                sh "mvn test"
              }
            }
          }
        }
        stage('build-qa') {
          when {
            branch 'release-*'
          }
          steps {
            echo "build ${params.CORES} to QA"
            dir ('my-app') {
              withMaven (maven: 'Maven Latest', jdk: 'JDK 1.8 Latest (Linux 64-bit)') {
                sh "mvn test"
              }
            }
          }
        }
        stage('build-dev') {
          when {
            branch 'dev'
          }
          steps {
            echo "build ${params.CORES} to Dev"
            dir ('my-app') {
              withMaven (maven: 'Maven Latest', jdk: 'JDK 1.8 Latest (Linux 64-bit)') {
                sh "mvn test"
              }
            }
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
    stage('final') {
      steps {
        echo "Go buy yourself ${params.flavor} Ice cream"
      }
    }
  }
  post {
    success {
      echo "I did it."
      helloWorld name: "Messenger"
    }
  }
}
