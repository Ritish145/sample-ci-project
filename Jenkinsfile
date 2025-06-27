pipeline {
    agent {
        docker {
            image 'node:18'
            args '-v /c/ProgramData/Jenkins/.jenkins/workspace/sample-ci-project:/workspace -w /workspace'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'  // Example command (adjust as needed)
            }
        }
    }
    post {
        always {
            cleanWs()  // Move inside a `node` block if still failing
        }
    }
}
