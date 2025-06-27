pipeline {
    agent any
    options {
        skipDefaultCheckout true  // Only if you want custom checkout
    }
    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    extensions: [
                        [$class: 'LocalBranch', localBranch: 'main'],  // Prevents detached HEAD
                        [$class: 'CloneOption', depth: 1, shallow: true]  // Faster clones
                    ],
                    userRemoteConfigs: [[
                        url: 'https://github.com/Ritish145/sample-ci-project.git'
                    ]]
                ])
            }
        }
        // Add your build/test stages here
    }
}
