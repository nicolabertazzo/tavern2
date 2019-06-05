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
    stage('run dspot') {
      steps {
        dspot(mvnHome: '/var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/maven339', numIterations: 2, amplifiers:'NumberLiteralAmplifier:AllLiteralAmplifiers')
      }
    }
  }
}
