pipeline {
    agent any
    
    options {
        skipDefaultCheckout(true)  // We'll handle checkout manually
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    environment {
        // Correct workspace path (note the .jenkins subfolder)
        WORKSPACE_PATH = "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\sample-ci-project"
        // For Docker containers
        DOCKER_WORKSPACE = "/workspace"
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Manually handle checkout with proper path
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: '*/main']],
                        extensions: [
                            [$class: 'RelativeTargetDirectory', 
                             relativeTargetDir: "${env.WORKSPACE}"],
                            [$class: 'CloneOption',
                             depth: 1,
                             shallow: true]
                        ],
                        userRemoteConfigs: [[
                            url: 'https://github.com/Ritish145/sample-ci-project.git'
                            // Remove credentialsId if not needed
                        ]]
                    ])
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Verify correct workspace
                    bat """
                        echo "Building in ${env.WORKSPACE}"
                        cd /d "${env.WORKSPACE}"
                        dir
                    """
                    
                    // Example Docker integration
                    docker.withRegistry('https://registry.hub.docker.com') {
                        docker.image('node:18').inside("-v ${env.WORKSPACE}:${env.DOCKER_WORKSPACE}") {
                            sh 'npm install'
                            sh 'npm run build'
                        }
                    }
                }
            }
        }

        stage('Test') {
            steps {
                bat """
                    cd /d "${env.WORKSPACE}"
                    echo "Running tests..."
                """
            }
        }
    }

    post {
        always {
            cleanWs(
                cleanWhenAborted: true,
                cleanWhenFailure: true,
                cleanWhenSuccess: true
            )
        }
    }
}
