pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                echo "Cloning GitHub Repository..."
                git branch: 'main',
                    url: 'https://github.com/shiva-6300/practice-project.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate

                pip install --upgrade pip
                pip install flask gunicorn
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                . venv/bin/activate

                pkill -f "gunicorn.*app:app" || true

                nohup gunicorn \
                --bind 0.0.0.0:5000 \
                --workers 2 \
                app:app > app.log 2>&1 &

                sleep 5

                echo "Application Started Successfully"
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment Successful!"
            echo "Open: http://65.2.168.165:5000"
        }

        failure {
            echo "Deployment Failed!"
        }
    }
}
