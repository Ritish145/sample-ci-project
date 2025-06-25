pipeline {
  agent {
    docker { image 'node:18' }
  }

  stages {
    stage('Build') {
      steps {
        echo 'Installing dependencies...'
        bat 'npm install'    // <-- Windows batch, causes error
      }
    }
    stage('Test') {
      steps {
        echo 'Running tests...'
        bat 'npm test'       // <-- Windows batch, causes error
      }
    }
  }
}
