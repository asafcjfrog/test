pipeline {
    agent any // Use your agent here with installed package manager (npm, go, python etc.)

    environment {
        // Corrected assignments with the correct syntax
        JF_GIT_REPO = "${env.JOB_NAME.split('/')[1]}"
        JF_GIT_PULL_REQUEST_ID = "${env.CHANGE_ID}"
        JF_GIT_OWNER = "${env.CHANGE_AUTHOR}"
        TRIGGER_KEY = "${env.CHANGE_TITLE}"
        // Add other environment variables as necessary
    }

    stages {
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
                    echo $JF_GIT_REPO
                    echo $JF_GIT_PULL_REQUEST_ID
                    echo $JF_GIT_OWNER
                    echo $TRIGGER_KEY
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
