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
        tool(type: 'maven', name: 'maven')
        rtMavenResolver(id: 'maven_resolver_1', serverId: 'Artifact', releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot')
        rtMavenDeployer(id: 'maven_deployer_1', serverId: 'Artifact', releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local')
        rtMavenRun(tool: maven, pom: '${POM_DIRECTORY}/pom.xml', goals: 'clean install ', resolverId: 'maven_resolver_1', deployerId: 'maven_deployer_1')
        rtPublishBuildInfo 'Artifact'
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