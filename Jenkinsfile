pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Preparing workspace'
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
        stage('Compliance Test') {
          steps {
            echo 'Initiating Compliance test'
            echo 'Compliance test complete'
          }
        }
        stage('QA Test') {
          steps {
            echo 'Initiating QA test'
            echo 'QA test complete'
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
}