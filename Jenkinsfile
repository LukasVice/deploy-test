pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        disableConcurrentBuilds()
    }

    properties([
    parameters([
        [$class: 'ChoiceParameter', choiceType: 'PT_SINGLE_SELECT', description: 'Select a choice', filterLength: 1, filterable: true, name: 'choice1', randomName: 'choice-parameter-7601235200970', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: 'return ["ERROR"]'], script: [classpath: [], sandbox: false, script: 'return[\'aaa\',\'bbb\']']]],
        [$class: 'CascadeChoiceParameter', choiceType: 'PT_SINGLE_SELECT', description: 'Active Choices Reactive parameter', filterLength: 1, filterable: true, name: 'choice2', randomName: 'choice-parameter-7601237141171', referencedParameters: 'choice1', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: 'return ["error"]'], script: [classpath: [], sandbox: false, script: 'if(choice1.equals("aaa")){return [\'a\', \'b\']} else {return [\'aaaaaa\',\'fffffff\']}']]]
    ])
])

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
