pipeline {
    agent any
    
    // Global pipeline configuration
    options {
        skipDefaultCheckout(false)
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }

    // Tools configuration (matches Jenkins Global Tools)
    tools {
        git 'Default'  // Must match your configured Git installation name
    }

    environment {
        // Custom environment variables
        PROJECT_DIR = "C:\\ProgramData\\Jenkins\\workspace\\sample-ci-project"
        DOCKER_WORKSPACE = "/workspace"  // Linux path for containers
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    extensions: [
                        [$class: 'CloneOption', 
                         depth: 1, 
                         shallow: true,
                         noTags: true,
                         honorRefspec: true],
                        [$class: 'LocalBranch',
                         localBranch: 'main'],
                        [$class: 'RelativeTargetDirectory',
                         relativeTargetDir: 'src']
                    ],
                    userRemoteConfigs: [[
                        url: 'https://github.com/Ritish145/sample-ci-project.git',
                        credentialsId: 'github-creds'  // From Jenkins credentials store
                    ]]
                ])
            }
        }

        stage('Build') {
            steps {
                script {
                    // Windows batch commands
                    bat """
                        echo "Building in ${PROJECT_DIR}"
                        cd "${PROJECT_DIR}"
                        dir  # Verify contents
                    """
                    
                    // Example for Docker builds
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-creds') {
                        docker.image('node:18').inside("""
                            -v "${PROJECT_DIR}:${DOCKER_WORKSPACE}"
                            -w "${DOCKER_WORKSPACE}"
                        """) {
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
                    cd "${PROJECT_DIR}"
                    echo "Running tests..."
                    # Add your test commands here
                """
            }
        }
    }

    post {
        always {
            script {
                // Clean workspace while preserving context
                node('master') {
                    cleanWs(
                        cleanWhenAborted: true,
                        cleanWhenFailure: true,
                        cleanWhenNotBuilt: true,
                        cleanWhenSuccess: true,
                        deleteDirs: true
                    )
                }
            }
        }
        success {
            slackSend(
                color: 'good',
                message: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
            )
        }
        failure {
            slackSend(
                color: 'danger',
                message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
            )
        }
    }
}
