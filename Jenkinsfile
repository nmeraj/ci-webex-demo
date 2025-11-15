pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out source code from Git..."
                checkout scm
            }
        }

        stage('Create venv') {
            steps {
                echo "Creating Python virtual environment..."
                sh "python -m venv venv"
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing dependencies into venv..."
                sh "./venv/bin/pip install -r requirements.txt"
            }
        }

        stage('Run Tests') {
            steps {
                echo "Running pytest in venv with PYTHONPATH set..."
                sh "PYTHONPATH=. ./venv/bin/pytest"
            }
        }
    }
}
