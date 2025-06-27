pipeline {
  agent {
    docker {
      image 'node:18'
      args '-v C:/ProgramData/Jenkins/.jenkins/workspace/sample-ci-project:/workspace'  // Mount Windows path to /workspace in container
      reuseNode true
    }
  }

  environment {
    WORKSPACE = '/workspace'   // Set WORKSPACE inside container to /workspace
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
