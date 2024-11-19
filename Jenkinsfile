pipeline {
    agent any

    stages {
        stage('Setup') {
            steps {
                // Ensure pipenv is installed
                sh 'pip install pipenv || true'
            }
        }
        stage('Build') {
            steps {
                sh 'pipenv --python python3 sync'
            }
        }
        stage('Test') {
            steps {
                sh 'pipenv run pytest'
            }
        }
        stage('Package') {
            when {
                anyOf { branch 'master'; branch 'release' }
            }
            steps {
                // Package files into a zip archive locally
                sh 'zip -r sbdl.zip lib'
            }
        }
        stage('Release') {
            when {
                branch 'release'
            }
            steps {
                // Simulate deployment to a release directory locally
                sh '''
                mkdir -p ~/sbdl-qa
                cp sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh /Users/abror/Documents/personal/learn/repo/sparks/SBDL_workspace/SBDL/conf/ ~/sbdl-qa/
                '''
            }
        }
        stage('Deploy') {
            when {
                branch 'master'
            }
            steps {
                // Simulate deployment to a production directory locally
                sh '''
                mkdir -p ~/sbdl-prod
                cp sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh /Users/abror/Documents/personal/learn/repo/sparks/SBDL_workspace/SBDL/conf/ ~/sbdl-prod/
                '''
            }
        }
    }
}
