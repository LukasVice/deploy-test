pipeline {
    agent any
    
    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        disableConcurrentBuilds()
    }
    
    parameters {
        booleanParam(defaultValue: false, description: '', name: 'DEPLOY_STAGING')
    }
    
    stages {
        stage('build') {
            steps {
                sh 'echo Success'
                
                script {
                    echo "Url: ${pullRequest.url}"
                    echo "State: ${pullRequest.state}"
                    echo "Statuses: ${pullRequest.statuses}"
                    echo "Mergeable: ${pullRequest.mergeable}"
                }
            }
        }
    }
}
