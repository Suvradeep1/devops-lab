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
    }
}