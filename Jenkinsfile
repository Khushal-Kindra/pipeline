pipeline {
    agent any
    environment {
        IMAGE_NAME = "khushal975/flask-app"
        CONTAINER_REGISTRY = "docker.io" // Change if using a private registry
    }
    options {
        timestamps() // Adds timestamps to logs
    }
    stages {
        stage('Remove Existing Container') {
            steps {
                sh 'docker rm xyz || true'
            }
        }
        stage('Build') {
            steps {
                echo 'Building the Docker image...'
                sh 'docker build -t $IMAGE_NAME .'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'docker run $IMAGE_NAME echo "Test successful!"'
            }
        }
        stage('Push Image') {
            steps {
                echo 'Logging into Docker registry...'
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
                echo 'Pushing Docker image to registry...'
                sh 'docker push $IMAGE_NAME'
            }
        }
        stage('Verify Docker Image Pull') {
            steps {
                echo 'Pulling and Running the Docker Image to Verify...'
                sh 'docker image rm $IMAGE_NAME || true' // Remove local image to test pulling
                sh 'docker pull $IMAGE_NAME'
                sh 'docker run --name xyz $IMAGE_NAME echo "Image Pulled and Running Successfully!"'
            }
        }
    }
    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline execution failed!'
        }
    }
}
