pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
    }
    
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

        stage('Setup virtual environment') {
            steps {
                script {
                    // Setup virtual environment
                    echo 'Setup virtual environment...'
                    sh '''
                        python -m venv ${VENV_DIR}
                        . ${VENV_DIR}/bin/activate
                        pip install --upgrade pip
                        pip install -e .
                    '''
                }
            }
        }

        stage('Linting code') {
            steps {
                script {
                    // Linting code
                    echo 'Linting code...'
                    sh '''
                        set -e
                        . ${VENV_DIR}/bin/activate
                        pylint application.py main.py --output=pylint-report.txt --exit-zero || echo "Pylint stage completed"
                        flake8 application.py main.py --ignore=E501,E302 --output-file=flake8-report.txt || echo "Flake8 stage completed"
                        black application.py main.py || echo "Black stage completed"
                    '''
                }
            }
        }

        stage('Trivy Scanning') {
            steps {
                script {
                    // Trivy scanning
                    echo 'Trivy scanning...'
                    sh "trivy fs ./ --format table -o trivy-fs-report.html"
                }
            }
        }

        
    }
}