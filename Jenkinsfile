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
                set -e

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
                set -e

                . venv/bin/activate

                echo "Stopping old Gunicorn..."
                pkill -f gunicorn || true

                echo "Starting Gunicorn..."
                BUILD_ID=dontKillMe nohup gunicorn \
                    --bind 0.0.0.0:5000 \
                    --workers 2 \
                    app:app > app.log 2>&1 &

                sleep 5

                echo "Checking Gunicorn Process..."
                ps -ef | grep gunicorn || true

                echo "Checking Port 5000..."
                ss -tulnp | grep 5000 || true
                '''
            }
        }
    }

    post {
        success {
            echo "======================================"
            echo "Deployment Successful!"
            echo "Open in Browser:"
            echo "http://65.2.168.165:5000"
            echo "======================================"
        }

        failure {
            echo "======================================"
            echo "Deployment Failed!"
            echo "======================================"
        }

        always {
            echo "Pipeline Finished"
        }
    }
}
