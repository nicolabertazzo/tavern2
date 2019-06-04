pipeline {
  agent any
  stages {
    stage('Compile') {
      steps {
        withMaven(maven: 'maven3', jdk: 'JDK8') {
          sh 'mvn clean compile'
        }

      }
    }
    stage('Unit Test') {
      steps {
        withMaven(maven: 'maven3', jdk: 'JDK8') {
          sh 'mvn test'
        }

      }
    }
    stage('Test your tests') {
      steps {
        withMaven(maven: 'maven3', jdk: 'JDK8') {
          sh 'mvn eu.stamp-project:pitmp-maven-plugin:1.3.6:descartes -DoutputFormats=HTML'
        }

        publishHTML([
                    allowMissing: false,
                    alwaysLinkToLastBuild: false,
                    keepAll: true,
                    reportDir: 'target/pit-reports',
                    reportFiles: '**/index.html',
                    reportName: "Pit Decartes"
                ])
        }
      }
      stage('') {
        steps {
          dspot(mvnHome: '${M2_HOME}')
        }
      }
    }
    environment {
      GIT_URL = sh (script: 'git config remote.origin.url', returnStdout: true).trim().replaceAll('https://','')
    }
  }