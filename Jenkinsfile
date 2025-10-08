pipeline {
    agent any

    environment {
        APP_NAME = "nodejs-cicd"
        IMAGE_NAME = "nodejs-cicd:latest"
        CONTAINER_NAME = "nodejs-cicd-container"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Cloning source code..."
                git branch: 'main', url: 'https://github.com/shrikantdayma/nodejs-cicd.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing npm packages..."
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                echo "Running Jest tests..."
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t ${IMAGE_NAME} .'
            }
        }

        stage('Deploy Container') {
            steps {
                echo "Deploying Docker container..."
                // Stop old container if running
                sh 'docker rm -f ${CONTAINER_NAME} || true'
                // Run new container
                sh 'docker run -d --name ${CONTAINER_NAME} -p 3000:3000 ${IMAGE_NAME}'
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful! Visit http://localhost:3000"
        }
        failure {
            echo "❌ Build or test failed."
        }
    }
}

