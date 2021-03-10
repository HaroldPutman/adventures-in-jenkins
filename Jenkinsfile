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
  triggers {
    cron( "H/5 * * * *" )
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
            sh "git tag -a ${env.BRANCH_NAME}-${env.BUILD_NUMBER} -m \"Tagged by Jenkins\""
            // sh "git push --tags"
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
          def event = currentBuild?.getBuildCauses()[0]?.event?.event?.toString()
          def causeClass = currentBuild?.getBuildCauses()[0]?._class
          def desc = currentBuild?.getBuildCauses()[0]?.shortDescription
          def user = currentBuild?.getBuildCauses()[0]?.userId
          def changes = currentBuild.changeSets.size()
          echo event
          echo "Cause class: ${causeClass}"
          echo "Short Description: ${desc}"
          echo "User id: ${user}"
          echo "Changes: ${changes}"
          currentBuild.description = "Built ${params.CORES}"
        }
      }
    }
  }
  post {
    success {
      helloWorld name: "Messenger"
    }
  }
}
