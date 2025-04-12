pipeline {
    agent any
    
    stages {
        stage('Cloning from github repo') {
            steps {
                script {
                    // cloning github repo
                    echo 'Cloning from github repo...'
                    checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'mlops-github-token', url: 'https://github.com/kishoremc/Mlops_project.git']])
                }
            }
        }
    }
}