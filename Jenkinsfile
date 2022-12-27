def githubUrl = "https://github.com/myo044/JenkinsDeploysToTomcatProject"
def tomcatServerUrl = "http://192.168.0.200:8085"
def tomcatCredentialsId = "8530a204-199e-4354-9ea2-3f03d805a168"
def tomcatContextPath = "/TomcatApp"
  void setBuildStatus(String message, String state) {
    step([
        $class: "GitHubCommitStatusSetter",
        reposSource: [$class: "ManuallyEnteredRepositorySource", url: "${githubUrl}"],
        contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "jenkins/build-status"],
        errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
        statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
    ]);
  }
pipeline {
  agent any
  tools {
    maven 'Maven3.8.6'
  }
  triggers{
    pollSCM('* * * * *')
  }
  stages {
    stage ('Git') {
      steps{
        echo "Git step in process"
        git "${githubUrl}"
        echo "Git step is finished"
      }
    }
    stage ('Build') {
      steps {
        echo "Build in process"
        sh 'mvn clean package'
        echo "Build is finished"
      }
    }
    stage ('Deploy') {
      steps {
        echo "Deploying is in process"
        script {
          deploy adapters: [tomcat9(credentialsId: "${tomcatCredentialsId}", path: '', url: "${tomcatServerUrl}")], contextPath: "${tomcatContextPath}", onFailure: false, war: 'target/*.war'
        }
        echo "Deploy is finished"
      }
    }
  }
  post{
    success{
      setBuildStatus("Build succeeded", "SUCCESS")
    }
    failure{
      setBuildStatus("Build failed", "FAILURE")
    }
  }
}
