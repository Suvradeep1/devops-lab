pipeline {
    agent any
    
    environment {
        APP_NAME = 'my-web-app'
        CONTAINER_NAME = 'live-web-app'
        HOST_PORT = '8081'
    }

    stages {
        stage('1. Quality Gate & Code Check') {
            steps {
                echo "=== Running Quality Checks for Build #${BUILD_NUMBER} ==="
                sh '''
                    # Check 1: File existence
                    if [ ! -f index.html ]; then
                        echo "ERROR: index.html is missing!"
                        exit 1
                    fi

                    # Check 2: Non-empty file check
                    if [ ! -s index.html ]; then
                        echo "ERROR: index.html is empty!"
                        exit 1
                    fi

                    echo "Quality Gate Passed: Required files present and valid."
                '''
            }
        }
        
        stage('2. Build Dynamic Image') {
            steps {
                echo "Building image: ${env.APP_NAME}:${BUILD_NUMBER}"
                sh "docker build -t ${env.APP_NAME}:${BUILD_NUMBER} ."
            }
        }
        
        stage('3. Verify Built Artifact') {
            steps {
                echo "Verifying image ${env.APP_NAME}:${BUILD_NUMBER} in local registry..."
                sh "docker images ${env.APP_NAME}:${BUILD_NUMBER}"
            }
        }

        stage('4. Deploy Application') {
            steps {
                echo "Deploying container ${env.CONTAINER_NAME} with version ${BUILD_NUMBER}..."
                sh "docker rm -f ${env.CONTAINER_NAME} || true"
                sh "docker run -d --name ${env.CONTAINER_NAME} -p ${env.HOST_PORT}:80 ${env.APP_NAME}:${BUILD_NUMBER}"
            }
        }
    }

    post {
        always {
            echo "Pipeline run completed for Build #${BUILD_NUMBER}."
        }
        failure {
            echo "ALERT: Pipeline failed! Deployment aborted. Previous container version remains live."
        }
    }
}