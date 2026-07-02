pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                checkout scm
            }
        }

        stage('Setup Environment') {
            steps {
                echo 'Creating virtual environment and installing dependencies...'
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running unit tests with pytest...'
                sh '''
                    . venv/bin/activate
                    pytest test_app.py -v
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
