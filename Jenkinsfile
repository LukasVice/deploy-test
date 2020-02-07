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
                    echo "Mergeable: ${pullRequest.mergeable}"
                }
            }
        }
    }
}
