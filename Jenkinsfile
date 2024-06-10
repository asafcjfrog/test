pipeline {
    agent any // Use your agent here with installed package manager (npm, go, python etc.)

    environment {
        // Corrected assignments with the correct syntax
        JF_GIT_REPO = "${env.GIT_URL.tokenize('/')[-1].replace('.git', '')}"
        JF_GIT_PULL_REQUEST_ID = "${env.CHANGE_ID}"
        JF_GIT_OWNER = "${env.CHANGE_AUTHOR}"
        TRIGGER_KEY = "opened" // Assuming we are interested in the PR creation event
    }

    stages {
        
        stage('Print Environment Variables') {
            steps {
                script {
                    // Print individual environment variables
                    echo "JF_GIT_REPO: ${env.JF_GIT_REPO}"
                    echo "JF_GIT_PULL_REQUEST_ID: ${env.JF_GIT_PULL_REQUEST_ID}"
                    echo "JF_GIT_OWNER: ${env.JF_GIT_OWNER}"
                    echo "TRIGGER_KEY: ${env.TRIGGER_KEY}"
                    
                    // Print all environment variables
                    echo "All Environment Variables:"
                    env
                }
            }
        }
        stage('Verify trigger') {
            steps {
                script {
                    if (env.CHANGE_ID) {
                        echo "This is a PR build"
                    } else {
                        echo "This is not a PR build"
                        error('Aborting the build as this is not a PR build.')
                    }
                }
            }
        }

        stage('Download Frogbot') {
            steps {
                script {
                    if (env.JF_RELEASES_REPO == "") {
                        sh """ curl -fLg "https://releases.jfrog.io/artifactory/frogbot/v2/latest/getFrogbot.sh" | sh"""
                    } else {
                        sh "curl -fLg '${env.JF_URL}/artifactory/${env.JF_RELEASES_REPO}/artifactory/frogbot/v2/latest/getFrogbot.sh' | sh"
                    }
                }
            }
        }

        stage('Scan Pull Request') {
            steps {
                sh "./frogbot scan-pull-request"
            }
        }
    }
}
