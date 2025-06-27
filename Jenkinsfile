pipeline {
    agent any
    tools {
        git 'Default'  // Must match the name you configured above
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        // ... other stages
    }
}
