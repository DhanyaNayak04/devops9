pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Creating virtual environment and installing dependencies...'
                sh 'python3 --version || echo "Python not found"'
                sh 'python3 -m venv venv'
                sh './venv/bin/pip install --upgrade pip'
                sh './venv/bin/pip install -r python-flask-app/requirements.txt'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                // ...existing code...
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh './venv/bin/python -m unittest discover -s python-flask-app'
                sh "mkdir -p ${env.WORKSPACE}/python-app-deploy"
                sh "cp ${env.WORKSPACE}/python-flask-app/app.py ${env.WORKSPACE}/python-app-deploy/"
            }
        }
        stage('Run Application') {
            steps {
                echo 'Running application...'
                sh "nohup ./venv/bin/python ${env.WORKSPACE}/python-app-deploy/app.py > ${env.WORKSPACE}/python-app-deploy/app.log 2>&1 &"
                sh "echo \$! > ${env.WORKSPACE}/python-app-deploy/app.pid"
            }
        }
        stage('Test Application') {
            steps {
                echo 'Testing application...'
                sh "./venv/bin/python ${env.WORKSPACE}/python-flask-app/test_app.py"
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for more details.'
        }
    }
}