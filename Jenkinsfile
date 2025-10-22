pipeline {
    agent any

    environment {
        // ✅ Add Python to PATH (update path if needed)
        PYTHON_HOME = 'C:\\Users\\91879\\AppData\\Local\\Programs\\Python\\Python310'
        PATH = "${env.PYTHON_HOME};${env.PYTHON_HOME}\\Scripts;${env.PATH}"
    }

    stages {

        stage('Run Selenium Tests with pytest') {
            steps {
                echo "Running Selenium Tests using pytest"
                
                // ✅ Create & activate a virtual environment for isolation
                bat '''
                python --version
                python -m venv venv
                call venv\\Scripts\\activate
                python -m pip install --upgrade pip
                pip install -r requirements.txt
                start /B python app.py
                ping 127.0.0.1 -n 5 > nul
                pytest -v --maxfail=1 --disable-warnings
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image"
                bat "docker build -t fazilabegum/week12:t5 ."
            }
        }

        stage('Docker Login') {
            steps {
                echo "Docker Login"
                bat 'docker login -u fazilabegum -p admin@123'
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                echo "Push Docker Image to Docker Hub"
                bat "docker push fazilabegum/week12:t5"
            }
        }

        stage('Deploy to Kubernetes') { 
            steps { 
                echo "Deploying to Kubernetes"
                bat 'kubectl apply -f deployment.yaml --validate=false' 
                bat 'kubectl apply -f service.yaml' 
            } 
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Please check the logs.'
        }
    }
}
