pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Creating virtual environment and installing dependencies...'
                sh 'pip install -r python-flask-app/requirements.txt'
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
                sh 'python -m unittest discover -s python-flask-app'
                sh "mkdir -p ${env.WORKSPACE}/python-app-deploy"
                sh "cp ${env.WORKSPACE}/python-flask-app/app.py ${env.WORKSPACE}/python-app-deploy/"
            }
        }
        stage('Run Application') {
            steps {
                echo 'Running application...'
                sh "nohup python ${env.WORKSPACE}/python-app-deploy/app.py > ${env.WORKSPACE}/python-app-deploy/app.log 2>&1 &"
                sh "echo \$! > ${env.WORKSPACE}/python-app-deploy/app.pid"
            }
        }
        stage('Test Application') {
            steps {
                echo 'Testing application...'
                sh "python ${env.WORKSPACE}/python-flask-app/test_app.py"
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