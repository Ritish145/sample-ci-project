pipeline {
  agent {
    docker {
      image 'node:18'
      args '-v /C/ProgramData/Jenkins/.jenkins/workspace/sample-ci-project:/workspace'
      reuseNode true
    }
  }

  environment {
    // Override the default workspace directory inside the container
    WORKSPACE = '/workspace'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install Dependencies') {
      steps {
        dir('/workspace') {
          sh 'npm install'
        }
      }
    }

    stage('Test') {
      steps {
        dir('/workspace') {
          sh 'npm test'
        }
      }
    }
  }
}
