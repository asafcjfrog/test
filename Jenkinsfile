pipeline {
    agent any // Use your agent here with installed package manager (npm, go, python etc.)

    environment {
        // Corrected assignments with the correct syntax
        JF_GIT_REPO = "${env.GIT_URL.tokenize('/')[-1].replace('.git', '')}"
        JF_GIT_PULL_REQUEST_ID = "${env.CHANGE_ID}"
        JF_GIT_OWNER = "${env.CHANGE_AUTHOR}"
        TRIGGER_KEY = "opened" // Assuming we are interested in the PR creation event
        
        JF_GIT_PROVIDER = "github"
        JF_URL = credentials("JF_URL")
        JF_USER = credentials("JF_USER")
        JF_PASSWORD = credentials("JF_PASSWORD")
        JF_GIT_TOKEN = credentials("JF_GIT_TOKEN")
        JF_RELEASES_REPO= ""
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
                    sh 'printenv'
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
                    sh """ curl -fLg "https://releases.jfrog.io/artifactory/frogbot/v2/[RELEASE]/getFrogbot.sh" | sh"""
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
