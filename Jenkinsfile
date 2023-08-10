pipeline {
    agent any
    
    stages {
        stage('Validation') {
            steps {
                script {
                    sh 'cd /var/lib/jenkins/jobs/status-check-decl-pipe/workspace'
                    sh 'composer install'
                }
            }
        }
    }
post {
        success {
            script {
                setBuildStatus("Build succeeded", "SUCCESS")
            }
        }

        failure {
            script {
                setBuildStatus("Build failed", "FAILURE")
            }
        } 
    }
}

void setBuildStatus(String message, String state) {
    script {
        step([
            $class: "GitHubCommitStatusSetter",
            reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/nikhilP-addweb/status-check-decla"],
            contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
            errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
            statusResultSource: [$class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]]]
        ])
    }
}
