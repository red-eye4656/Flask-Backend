pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/red-eye4656/Flask-Backend.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv .venv
                    .venv/bin/pip install --upgrade pip
                    .venv/bin/pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    .venv/bin/python -m py_compile app.py
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    pm2 restart flask-backend || \
                    pm2 start app.py --name flask-backend \
                    --interpreter "$(pwd)/.venv/bin/python"

                    pm2 save
                '''
            }
        }
    }

    post {
        success {
            echo 'Flask deployment successful!'
        }
        failure {
            echo 'Flask deployment failed!'
        }
    }
}
