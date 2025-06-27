<<<<<<< HEAD
pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        echo 'Building on main branch...'
      }
    }
  }
}
=======
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
        checkout scm
      }
    }

    stage('Install Dependencies') {
      steps {
        bat 'npm install'
      }
    }

    stage('Test') {
      steps {
        bat 'npm test'
      }
    }
  }
}
>>>>>>> 8ec74d0
