pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Check Python') {
            steps {
                sh 'python3 --version'
                sh 'pip3 --version'
            }
        }

        stage('Create Virtual Environment') {
            steps {
                sh '''
                python3 -m venv venv
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements-ci.txt
                '''
            }
        }

        stage('Run Test') {
            steps {
                sh '''
                . venv/bin/activate

                if [ -d tests ]; then
                    pytest
                else
                    echo "Tidak ada folder tests."
                fi
                '''
            }
        }

        stage('Check FastAPI') {
            steps {
                sh '''
                . venv/bin/activate

                python -c "import fastapi; print('FastAPI OK')"
                python -c "import uvicorn; print('Uvicorn OK')"
                '''
            }
        }

        stage('Pipeline Success') {
            steps {
                echo '========================================='
                echo 'Pipeline berhasil dijalankan!'
                echo '========================================='
            }
        }

    }

    post {
        always {
            echo 'Pipeline selesai.'
        }

        success {
            echo 'SUCCESS'
        }

        failure {
            echo 'FAILED'
        }
    }
}
