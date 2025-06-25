pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building project...'
                sh 'npm install' // or pip install -r requirements.txt
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test'    // or pytest tests/
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
            }
        }
    }
}
