pipeline {
  agent {
    label "java"
  }
  stages {
    stage('Build') {
      steps{
        sh 'mvn clean install'
      }
    }  
    stage('jacoco') {
      steps{
        jacoco()
      }
    }
  }
}
