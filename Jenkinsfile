def buildEnvironments = env.BRANCH_NAME == 'master' ? 'STAGING,LIVE' : 'STAGING'

properties([
    parameters([
        extendedChoice(
            description: 'Environments to build',
            name: 'ENVIRONMENTS',
            type: 'PT_CHECKBOX',
            value: buildEnvironments
        )
    ])
])

pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        disableConcurrentBuilds()
    }

    environment {
        GITHUB_TOKEN = credentials('ae612607-f01d-43b3-8b51-77beb598b215')
    }

    stages {
        stage('check') {
            steps {
                script {
                    def mergeable = sh(
                        script: 'docker run --rm -v $(pwd):/src -w /src -e GITHUB_TOKEN --entrypoint /bin/sh abergmeier/hub:2.12.8 -c "hub api repos/{owner}/{repo}/pulls/3 -t | awk \\"/^\\\\.mergeable\\\\t/ { print \\\\\\$2 }\\""',
                        returnStdout: true
                    )
                    if (!mergeable.toBoolean()) {
                        error('Pull request is not mergeable!')
                    }
                }
            }
        }

        stage('build') {
            steps {
                sh 'echo Success'

                script {
                    echo "Url: ${pullRequest.url}"
                    echo "State: ${pullRequest.state}"
                    echo "Statuses: ${pullRequest.statuses[0].state}"
                    echo "Mergeable: ${pullRequest.mergeable}"
                    echo "Merged: ${pullRequest.merged}"
                }
            }
        }
    }
}
