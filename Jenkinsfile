def deployEnvironments = env.BRANCH_NAME == 'master' ? 'STAGING,LIVE' : 'STAGING,GOG'

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
        stage('Build-Requirements') {
            steps {
                sh 'docker build -t github-hub:latest docker/hub'
            }
        }
        stage('Info') {
            when {
                expression { env.CHANGE_ID == null }
            }
            steps {
                echo "Branch Name: ${env.BRANCH_NAME}"
                echo "Deploy To: ${env.DEPLOY_TO}"
                script {
                    PR_ID = sh(
                        script: "docker run --rm -v \$(pwd):/src -e GITHUB_TOKEN github-hub:latest pr show -f %I",
                        returnStdout: true
                    ).trim()
                }
                echo "PR: ${PR_ID}"
            }
        }
        stage('Build') {
            steps {
                echo "-------- BUILD"
            }
        }
        stage('Test') {
            when {
                expression { env.CHANGE_ID != null }
            }
            steps {
                echo "-------- TEST"
            }
        }
        stage('Check PR mergeability') {
            when {
                expression { env.PR_ID != null }
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
        stage('Deploy') {
            when {
                allOf {
                    expression { env.DEPLOY_TO != null }
                    expression { env.PR_ID != null }
                }
            }
            steps {
                echo "PR: ${PR_ID}"
                echo "Execute build."
            }
        }
    }
}
