def deployEnvironments = env.BRANCH_NAME == 'master' ? 'STAGING,LIVE' : 'STAGING'

pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        disableConcurrentBuilds()
    }

    parameters {
        extendedChoice(name: 'DEPLOY_TO', type: 'PT_CHECKBOX', value: deployEnvironments)
    }

    environment {
        GITHUB_TOKEN = credentials('ae612607-f01d-43b3-8b51-77beb598b215')
    }

    stages {
        stage('Build') {
            steps {
                echo "${env}"
                echo "${env.DEPLOY_TO}"
                echo "Execute build."
            }
        }
        stage('Check PR mergeability') {
            when {
                allOf {
                    expression { env.DEPLOY_TO != null }
                    expression { env.CHANGE_ID != null }
                }
            }
            steps {
                script {
                    def mergeableState = sh(
                        script: "docker run --rm -v \$(pwd):/src -w /src -e GITHUB_TOKEN --entrypoint /bin/sh abergmeier/hub:2.12.8 -c \"hub api repos/{owner}/{repo}/pulls/$CHANGE_ID -t | awk \\\"/^\\\\.mergeable_state\\\\t/ { print \\\\\\\$2 }\\\"\"",
                        returnStdout: true
                    ).trim()
                    if (mergeableState != 'clean') {
                        error("Pull request is not mergeable! (State: '$mergeableState', should be 'clean')")
                    }
                }
            }
        }
    }
}
