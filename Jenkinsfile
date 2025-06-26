pipeline {
    agent {
        docker {
            image 'node:18'
            args '-v /C//ProgramData/Jenkins/.jenkins/workspace/sample-ci-project:/workspace'
            reuseNode true
        }
    }
    stages {
        stage('Build') {
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
