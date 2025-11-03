pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    extensions: [[$class: 'CleanBeforeCheckout']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/AgentR04/mlops-iris-demo.git'
                    ]]
                ])
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'python3 -m venv venv'
                sh '. venv/bin/activate && pip install --upgrade pip && pip install -r requirements.txt'
            }
        }

        stage('Retrain Model') {
            steps {
                sh '. venv/bin/activate && python retrain.py'
            }
        }

        stage('Deploy Model') {
            steps {
                echo 'Deployment stage placeholder — add deployment script here if needed.'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished (success or fail). Cleaning up workspace.'
            deleteDir()
        }
    }
}

