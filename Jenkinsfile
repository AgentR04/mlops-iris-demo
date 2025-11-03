pipeline {
    agent any

    environment {
        // Name of your Python virtual environment folder
        VENV = 'venv'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/AgentR04/mlops-iris-demo.git'
            }
        }

        stage('Setup Python Env') {
            steps {
                sh '''
                python3 -m venv ${VENV}
                . ${VENV}/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Retrain Model') {
            steps {
                sh '''
                . ${VENV}/bin/activate
                python retrain.py
                '''
            }
        }

        stage('Push Updated Model') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'github-token',   // 👈 Replace with your Jenkins credential ID
                    usernameVariable: 'USERNAME',
                    passwordVariable: 'TOKEN'
                )]) {
                    sh '''
                    git config --global user.email "jenkins@ml-server.com"
                    git config --global user.name "Jenkins Bot"

                    git add iris_model.pkl
                    git commit -m "Auto: Retrained model on $(date)" || echo "No changes to commit"

                    git push https://${USERNAME}:${TOKEN}@github.com/AgentR04/mlops-iris-demo.git main
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ Model retraining pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check logs for errors."
        }
    }
}

