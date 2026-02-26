pipeline {
    agent any

    environment {
        PYTHON_ENV = 'ml-env'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Environment') {
            steps {
                bat '''
                    python -m venv %PYTHON_ENV%
                    call %PYTHON_ENV%\\Scripts\\activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Train Model') {
            steps {
                bat '''
                    call %PYTHON_ENV%\\Scripts\\activate
                    python src\\train_sklearn.py
                '''
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'models/**/*', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Training completed successfully!'
        }
        failure {
            echo 'Training failed!'
        }
    }
}
