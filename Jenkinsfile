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
        stage('Build requirements') {
            steps {
                sh 'docker build -t github-hub:latest docker/hub'
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
                expression { env.CHANGE_ID == null && env.BRANCH_NAME != 'master' }
            }
            steps {
                script {
                    def PR_ID = sh(
                        script: "docker run --rm -v \$(pwd):/src -e GITHUB_TOKEN github-hub:latest pr show -f %I",
                        returnStdout: true
                    ).trim()
                    def mergeableState = sh(
                        script: "docker run --rm -v \$(pwd):/src -w /src -e GITHUB_TOKEN --entrypoint /bin/sh github-hub:latest -c \"hub api repos/{owner}/{repo}/pulls/$PR_ID -t | awk \\\"/^\\\\.mergeable_state\\\\t/ { print \\\\\\\$2 }\\\"\"",
                        returnStdout: true
                    ).trim()
                    if (mergeableState != 'clean' && mergeableState != 'unstable') {
                        error("Pull request is not mergeable! (State: '$mergeableState', should be 'clean' or 'unstable')")
                    }
                }
            }
        }
        stage('Deploy') {
            when {
                expression { env.DEPLOY_TO != '' && env.CHANGE_ID == null }
            }
            steps {
                echo "-------- DEPLOY"
            }
        }
    }
}
