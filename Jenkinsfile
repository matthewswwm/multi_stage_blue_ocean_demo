pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Preparing workspace'
        sh 'mvn -f ${env.pom_directory} clean install'
        echo 'Workspace Preparation complete'
      }
    }
    stage('Testing') {
      parallel {
        stage('SonarQube Test') {
          steps {
            echo 'Initiating SonarQube test'
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
    pom_directory = '/maven_test'
  }
}