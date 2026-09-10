```groovy
pipeline {
    agent any

    environment {
        IMAGE_NAME = "python-cicd-app"
        CONTAINER_NAME = "python-app"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/YOUR_USERNAME/python-cicd-app.git'
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    python3 -m pytest -v
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t ${IMAGE_NAME}:latest .
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                    docker stop ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Remove Old Container') {
            steps {
                sh '''
                    docker rm ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                    docker run -d \
                        --name ${CONTAINER_NAME} \
                        -p 5000:5000 \
                        ${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                    sleep 3

                    echo "Docker containers:"
                    docker ps

                    echo "Testing application:"
                    curl -f http://localhost:5000

                    echo ""
                    echo "Application is running successfully!"
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD pipeline completed successfully!'
        }

        failure {
            echo 'CI/CD pipeline failed!'
        }
    }
}
```

