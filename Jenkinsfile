def githubUrl = "https://github.com/myo044/JenkinsDeploysToTomcatProject"
def tomcatServerUrl = "http://192.168.43.151:8085"
def tomcatCredentialsId = "8530a204-199e-4354-9ea2-3f03d805a168"
def tomcatContextPath = "/TomcatApp"
pipeline {
  agent any
  tools {
    maven 'Maven3.8.6'
  }
  stages {
    stage ('Git') {
      steps{
        git "${githubUrl}"
      }
    }
    stage ('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage ('Deploy') {
      steps {
        script {
          deploy adapters: [tomcat9(credentialsId: "${tomcatCredentialsId}", path: '', url: "${tomcatServerUrl}")], contextPath: "${tomcatContextPath}", onFailure: false, war: 'target/*.war'
        }
      }
    }
  }
}