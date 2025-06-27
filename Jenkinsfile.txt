pipeline {
  agent {
    docker {
      image 'node:18'
      // Mount Windows path to /workspace inside container (use lowercase drive letter and forward slashes)
      args '-v /c/ProgramData/Jenkins/.jenkins/workspace/sample-ci-project:/workspace -w /workspace'
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
        // use sh, because we're inside a Linux container
        sh 'npm install'
      }
    }
    stage('Test') {
      steps {
        sh 'npm test'
      }
    }
  }
}
