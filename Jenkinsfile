@Library('stamp') _

pipeline {
  agent any
  stages {
    stage('Compile') {
      steps {
        withMaven(maven: 'maven3', jdk: 'JDK8') {
          sh "mvn clean compile"
        }
      }
    }

   stage('Unit Test') {
      steps {
        withMaven(maven: 'maven3', jdk: 'JDK8') {
          sh "mvn test"
        }
      }
    }

    stage ('Test your tests'){
      steps {
        withMaven(maven: 'maven3', jdk: 'JDK8') {
          sh "mvn eu.stamp-project:pitmp-maven-plugin:1.3.6:descartes -DoutputFormats=HTML"
        }
         publishHTML (target: [
          allowMissing: false,
          alwaysLinkToLastBuild: false,
          keepAll: true,
          reportDir: 'target/pit-reports',
          reportFiles: '**/index.html',
          reportName: "Pit Decartes"
      ])
      }
    }

    stage('Amplify') {
      steps {
        script {
          stamp.cloneLastStableVersion("oldVersion")
            if (fileExists("${WORKSPACE}/oldVersion/src")){
              withMaven(maven: 'maven3', jdk: 'JDK8') {  
                sh "mvn clean eu.stamp-project:dspot-diff-test-selection:list -Dpath-dir-second-version=${WORKSPACE}/oldVersion"
                sh "mvn eu.stamp-project:dspot-maven:amplify-unit-tests -Dpath-to-test-list-csv=testsThatExecuteTheChange.csv -Dverbose -Dtest-criterion=ChangeDetectorSelector -Dpath-to-properties=src/main/resources/tavern.properties -Damplifiers=NumberLiteralAmplifier -Diteration=2"
              }
            }
          }
        }
      }
    }
   environment {
    GIT_URL = sh (script: 'git config remote.origin.url', returnStdout: true).trim().replaceAll('https://','')
  }
}
