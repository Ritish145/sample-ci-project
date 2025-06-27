pipeline {
  agent {
    docker {
      image 'node:18'
      // Use Linux style paths with forward slashes and lowercase drive letter with preceding slash
      args '-v /c/ProgramData/Jenkins/.jenkins/workspace/sample-ci-project:/workspace'
      // Set working directory to the mounted folder inside container
      reuseNode true
      // Important: set -w to the same Linux path, NOT Windows style!
      // Remove the -w from args and specify working directory below
    }
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
