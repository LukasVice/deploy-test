properties([parameters([[$class: 'ChoiceParameter', choiceType: 'PT_MULTI_SELECT', description: '', filterLength: 1, filterable: false, name: 'ENVIRONMENT', randomName: 'choice-parameter-24339734020414409', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: ''], script: [classpath: [], sandbox: true, script: 'return [\'One\', \'Two\']']]]]), gitLabConnection('GitLab'), [$class: 'JiraProjectProperty']])

pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        disableConcurrentBuilds()
    }

    parameters {
        booleanParam(defaultValue: false, description: '', name: 'DEPLOY_STAGING')
        booleanParam(defaultValue: false, description: '', name: 'DEPLOY_MASTER')
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
