pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                checkout scm
            }
        }
stage('Debug') {
    steps {
        sh '''
            pwd
            ls -la
            which python3
            python3 --version
            which pip
            pip --version
        '''
    }
}
        stage('Setup Environment') {
    steps {
        sh '''
            python3 -m venv venv

            ls -la venv/bin

            ./venv/bin/python --version
            ./venv/bin/python -m pip --version

            ./venv/bin/python -m pip install -r requirements.txt
        '''
    }
        }
        stage('Run Tests') {
    steps {
        sh '''
            ./venv/bin/pytest test_app.py -v
        '''
    }
        }
        stage('Build') {
            steps {
                echo 'Simulating a build step (packaging the app)...'
                sh 'tar -czf app-build.tar.gz app.py requirements.txt'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'app-build.tar.gz', fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check the logs above.'
        }
        always {
            echo 'Cleaning up workspace...'
            sh 'rm -rf venv'
        }
    }
}
