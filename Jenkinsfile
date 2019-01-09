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
    stage('Testing') {
      parallel {
        stage('SonarQube Test') {
          steps {
            echo 'Initiating SonarQube test'
            sh 'mvn sonar:sonar -f ${POM_DIRECTORY}/pom.xml -Dlicense.skip=true'
            echo 'SonarQube test Complete'
          }
        }
        stage('Selenium Test') {
          steps {
            echo 'Initiating Selenium test'
            echo 'Selenium test complete'
          }
        }
      }
    }
    stage('JFrog Push') {
      steps {
        echo 'Starting JFrog push'
        script {
          def server = Artifactory.server "Artifactory Server"
          def buildInfo = Artifactory.newBuildInfo()
          def rtMaven = Artifactory.newMavenBuild()

          rtMaven.tool = 'maven'
          rtMaven.deployer server: server, releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local'

          buildInfo = rtMaven.run pom: '${POM_DIRECTORY}\\pom.xml', goals: 'clean install -Dv=${BUILD_NUMBER} -Dlicense.skip=true'
          buildInfo.env.capture = true
          buildInfo.name = ${JOB_NAME}
          server.publishBuildInfo buildInfo
        }

        echo 'JFrog push complete'
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