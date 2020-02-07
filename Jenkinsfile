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
                    if (pullRequest.mergeable) {
                        echo "Mergeable"
                    } else {
                        echo "Not mergeable"
                    }
                }
            }
        }
    }
}
