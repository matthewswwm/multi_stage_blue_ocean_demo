pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Initiating maven build'
        sh 'mvn -f ${POM_DIRECTORY}/pom.xml clean install -Dlicense.skip=true'
        echo 'Maven build complete'
      }
    }
    stage('Testing stage') {
      parallel {
        stage('SonarQube Test') {
          steps {
            echo 'Initiating SonarQube test'
            sh 'mvn sonar:sonar -f ${POM_DIRECTORY}/pom.xml -Dlicense.skip=true -Dsonar.host.url=http://54.169.216.235:8081'
            echo 'SonarQube test Complete'
          }
        }
        stage('test 1') {
          steps {
            sleep 20
          }
        }
        stage('test 2') {
          steps {
            sleep 10
          }
        }
      }
    }
    stage('Artefact Push') {
      parallel {
        stage('JFrog Push') {
          steps {
            echo 'Starting JFrog push'
            script {
              def server = Artifactory.server "artifactory"
              def buildInfo = Artifactory.newBuildInfo()
              def rtMaven = Artifactory.newMavenBuild()

              rtMaven.tool = 'maven'
              rtMaven.deployer server: server, releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local'

              buildInfo = rtMaven.run pom: 'jpetstore-6/pom.xml', goals: "clean install -Dlicense.skip=true"
              buildInfo.env.capture = true
              buildInfo.name = 'jpetstore-6'
              server.publishBuildInfo buildInfo
            }

            echo 'JFrog push complete'
          }
        }
        stage('Nexus Push') {
          steps {
            echo 'Starting Nexus Push'
            echo 'Nexus push complete'
          }
        }
      }
    }
    stage('Deploy prompt') {
      steps {
        input 'Deploy to Production?'
      }
    }
    stage('Deploy') {
      steps {
        echo 'Initiating Deployment'
        echo 'Deployment Complete'
      }
    }
  }
  tools {
    maven 'maven'
  }
  environment {
    POM_DIRECTORY = 'jpetstore-6'
  }
}