pipeline {
  agent {
    docker {
      image 'node:18'
      args '-v /c/ProgramData/Jenkins/.jenkins/workspace/sample-ci-project:/workspace -w /workspace'
      reuseNode true
    }
  }

  stages {
    stage('Checkout') {
      steps {
        // Explicit checkout with Git tool specified
        checkout([$class: 'GitSCM', 
                  branches: [[name: '*/main']], 
                  userRemoteConfigs: [[url: 'https://github.com/Ritish145/sample-ci-project.git']], 
                  gitTool: 'Default'])
      }
    }

    stage('Install Dependencies') {
      steps {
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
