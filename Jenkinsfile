pipeline {
  agent any
  tools {
    maven 'Maven3.8.6'
  }
  stages {
    stage ('Build') {
      steps {
        git 'https://github.com/myo044/JenkinsDeploysToTomcatProject'

        sh 'mvn clean package'
      }
    }
    stage ('Deploy') {
      steps {
        script {
          deploy adapters: [tomcat9(credentialsId: 'deployer', path: '', url: 'http://192.168.43.151:8085')], contextPath: '/pipeline', onFailure: false, war: 'target/*.war'
        }
      }
    }
  }
}