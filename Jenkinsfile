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
/*
    stage('SonarQube analysis') {
      steps{
        script {
          def scannerHome = tool 'sonar';
          withSonarQubeEnv('sonarqube') {
            sh """
              ${scannerHome}/bin/sonar-scanner \
              -Dsonar.projectKey=javawebappproject \
              -Dsonar.projectName=javawebappproject \
              -Dsonar.projectVersion=1.0 \
              -Dsonar.java.binaries='target/classes'
            """
          }
        }
      }
    }

    stage("Sonar Quality Gate Check") {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    script {
                        def qualityGate = waitForQualityGate()
                        if (qualityGate.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qualityGate.status}"
                        }
                    }
                } // End of timeout
            }
    }
*/
    stage('Upload to Nexus') {
      steps{
         nexusArtifactUploader artifacts: [[artifactId: 'SimpleWebApplication', classifier: '', file: 'target/SimpleWebApplication.war', type: 'war']], credentialsId: 'nexus_credentials', groupId: 'com.maven', nexusUrl: '15.206.160.152:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '9.1.14-SNAPSHOT'
      }
    }
    stage('Deploy to Tomcat') {
      steps{
        sh 'echo "Here we deploy the build to tomcat"'
        deploy adapters: [tomcat9(credentialsId: '9ae28e96-08b4-43bd-a07d-418c21df2572', path: '', url: 'http://13.127.33.65:8081/')], contextPath: null, war: '**/SampleWebApplication.war'
      }
    }
  }
}
