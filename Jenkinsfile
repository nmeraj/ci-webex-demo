pipeline {
    agent any

    environment {
        PYTHON = 'python3'
    }

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
                sh "${env.PYTHON} -m venv venv"
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

    post {
        success {
            echo "Sending SUCCESS notification to WebEx..."
            withCredentials([string(credentialsId: 'WEBEX_BOT_TOKEN', variable: 'WEBEX_TOKEN')]) {
                sh '''
                    cat <<EOF > payload.json
                    {
                      "roomId":"Y2lzY29zcGFyazovL3VybjpURUFNOnVzLXdlc3QtMl9yL1JPT00vMDQ1Y2IyMjAtYzI2My0xMWYwLTliNWQtYjk4ZTdjN2Q4Njdk",
                      "markdown": "✅ Build SUCCESS from Jenkins!"
                    }
                    EOF

                    curl -X POST \
                      -H "Authorization: Bearer ${WEBEX_TOKEN}" \
                      -H "Content-Type: application/json" \
                      -d @payload.json \
                      https://webexapis.com/v1/messages
                '''
            }
        }

        failure {
            echo "Sending FAILURE notification to WebEx..."
            withCredentials([string(credentialsId: 'WEBEX_BOT_TOKEN', variable: 'WEBEX_TOKEN')]) {
                sh '''
                    cat <<EOF > payload.json
                    {
                      "roomId":"Y2lzY29zcGFyazovL3VybjpURUFNOnVzLXdlc3QtMl9yL1JPT00vMDQ1Y2IyMjAtYzI2My0xMWYwLTliNWQtYjk4ZTdjN2Q4Njdk",
                      "markdown": "❌ Build FAILED from Jenkins!"
                    }
                    EOF

                    curl -X POST \
                      -H "Authorization: Bearer ${WEBEX_TOKEN}" \
                      -H "Content-Type: application/json" \
                      -d @payload.json \
                      https://webexapis.com/v1/messages
                '''
            }
        }
    }
}
