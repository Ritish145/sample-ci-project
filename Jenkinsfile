pipeline {
    agent any
    
    tools {
        git 'Default'  // Matches the name you configured in Global Tools
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[
                        credentialsId: 'your-credentials-id',  // From Jenkins credentials
                        url: 'https://github.com/Ritish145/sample-ci-project.git'
                    ]]
                ])
            }
        }
        // Add your other stages here
    }
}
