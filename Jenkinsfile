pipeline {
    agent any
    
    stages {
        stage('1. Validate Code') {
            steps {
                echo 'Checking repository files...'
                sh 'ls -la'
            }
        }
        
        stage('2. Build Docker Image') {
            steps {
                echo 'Building Docker container image locally...'
                sh 'docker build -t my-web-app:v1 .'
            }
        }
        
        stage('3. Verify Local Image') {
            steps {
                echo 'Checking built images on host...'
                sh 'docker images my-web-app:v1'
            }
        }

        stage('4. Deploy to Local Server') {
            steps {
                echo 'Deploying application container...'
                // Stop and remove any previously running container with the same name
                sh 'docker rm -f live-web-app || true'
                // Launch the updated app container on port 8081
                sh 'docker run -d --name live-web-app -p 8081:80 my-web-app:v1'
            }
        }
    }
}