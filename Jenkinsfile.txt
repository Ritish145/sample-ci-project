pipeline {
  agent {
    docker {
      image 'node:18'
      args '-v C:/ProgramData/Jenkins/.jenkins/workspace/sample-ci-project:/workspace'
      reuseNode true
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
          bat 'npm install'
        }
      }
    }

    stage('Test') {
      steps {
        dir('/workspace') {
          bat 'npm test'
        }
      }
    }
  }
}
