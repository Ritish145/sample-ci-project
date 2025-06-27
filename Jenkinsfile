pipeline {
    agent any
    
    options {
        skipDefaultCheckout(true)  // We'll handle checkout manually
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        retry(2)  // Retry failed builds once
    }

    environment {
        // Windows paths (double backslashes)
        WIN_WORKSPACE = "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\sample-ci-project"
        // Linux paths (for Docker)
        DOCKER_WORKSPACE = "/workspace"
        // Git configuration
        GIT_TOOL = 'Default'  // Must match Jenkins Global Tool config
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
                         noTags: true],
                        [$class: 'RelativeTargetDirectory',
                         relativeTargetDir: '.']
                    ],
                    userRemoteConfigs: [[
                        url: 'https://github.com/Ritish145/sample-ci-project.git'
                    ]]
                ])
            }
        }

        stage('Build') {
            steps {
                script {
                    // Verify workspace (Windows)
                    bat """
                        echo "Workspace: %WIN_WORKSPACE%"
                        cd /d "%WIN_WORKSPACE%"
                        dir
                    """

                    // Docker build (Linux container)
                    docker.withRegistry('https://registry.hub.docker.com') {
                        docker.image('node:18').inside(
                            "-v ${env.WIN_WORKSPACE}:${env.DOCKER_WORKSPACE} " +
                            "-w ${env.DOCKER_WORKSPACE}"
                        ) {
                            sh 'npm install'
                            sh 'npm run build'
                        }
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    docker.image('node:18').inside(
                        "-v ${env.WIN_WORKSPACE}:${env.DOCKER_WORKSPACE} " +
                        "-w ${env.DOCKER_WORKSPACE}"
                    ) {
                        sh 'npm test'
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean workspace but preserve important artifacts
            script {
                archiveArtifacts artifacts: '**/build/**/*', allowEmptyArchive: true
                junit '**/test-results/**/*.xml'  // If using JUnit reports
                cleanWs(
                    cleanWhenAborted: true,
                    cleanWhenFailure: true,
                    cleanWhenSuccess: true,
                    deleteDirs: true
                )
            }
        }
        success {
            slackSend(
                color: 'good',
                message: "SUCCESS: ${env.JOB_NAME} [${env.BUILD_NUMBER}]"
            )
        }
        failure {
            slackSend(
                color: 'danger',
                message: "FAILED: ${env.JOB_NAME} [${env.BUILD_NUMBER}]"
            )
        }
    }
}
