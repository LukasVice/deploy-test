pipeline {
    agent any
    
    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        disableConcurrentBuilds()
    }
    
    parameters {
        booleanParam(defaultValue: false, description: '', name: 'DEPLOY_STAGING')
        booleanParam(defaultValue: false, description: '', name: 'DEPLOY_MASTER')
        booleanParam(defaultValue: false, description: '', name: null)
    }
    
    stages {
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
