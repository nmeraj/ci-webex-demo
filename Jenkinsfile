pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
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
                echo "Running pytest in venv..."
                sh "./venv/bin/pytest"
            }
        }
    }
}
