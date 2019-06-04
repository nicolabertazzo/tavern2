pipeline {
  agent any
  stages {
    stage('Compile') {
      steps {
        withMaven(maven: 'maven339') {
          sh 'mvn clean package'
        }

      }
    }
  }
}