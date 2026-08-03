pipeline {
    agent any
    
    environment {
        DOCKER_USER = 'suvro321'
        APP_NAME = 'my-web-app'
        CONTAINER_NAME = 'live-web-app'
        HOST_PORT = '8081'
    }

    stages {
        stage('1. Quality Gate') {
            steps {
                sh '[ -f index.html ] && [ -s index.html ]'
            }
        }
        
        stage('2. Build Image') {
            steps {
                sh "docker build -t ${env.DOCKER_USER}/${env.APP_NAME}:${BUILD_NUMBER} ."
            }
        }

        stage('3. Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh "docker push ${env.DOCKER_USER}/${env.APP_NAME}:${BUILD_NUMBER}"
                }
            }
        }

        stage('4. Deploy Application') {
            steps {
                sh "docker rm -f ${env.CONTAINER_NAME} || true"
                sh "docker run -d --name ${env.CONTAINER_NAME} -p ${env.HOST_PORT}:80 ${env.DOCKER_USER}/${env.APP_NAME}:${BUILD_NUMBER}"
            }
        }
    }

    post {
        always {
            echo "Cleaning up local build image to save disk space..."
            sh "docker rmi ${env.DOCKER_USER}/${env.APP_NAME}:${BUILD_NUMBER} || true"
            sh "docker image prune -f"
        }
    }
}