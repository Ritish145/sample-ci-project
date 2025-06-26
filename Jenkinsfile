pipeline {
    agent {
        docker {
            image 'node:18'
            args '-u root:root'  // Optional: run as root user inside container
        }
    }
    stages {
        stage('Build') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test'
            }
        }
    }
}
