pipeline {
    agent any
    environment {
        IMAGE_NAME = 'SArreic/Teedy'
        IMAGE_TAG = 'latest'
    }
    stages{
        stage('Package') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [],
                userRemoteConfigs: [[url: 'https://github.com/SArreic/Teedy.git']])
                bat 'mvn -B -DskipTests clean package --fail-never'
            }
        }
        // Building Docker images
        stage('Building Image') {
                    steps {
                        bat "docker build -t ${DOCKER_IMAGE} ."
                    }
                }
        // Uploading Docker images into Docker Hub
        stage('Upload image') {
                    steps {
                        script {
                            // Login to Docker Hub
                            docker.withRegistry('https://registry.hub.docker.com', DOCKER_CREDENTIALS) {
                                // Build and push the Docker image
                                def image = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                                image.push()
                            }
                        }
                    }
                }
        //Running Docker container
        stage('Run containers'){
            steps{
                script{
                    // Pull the image from Docker Hub
                    docker.image("${IMAGE_NAME}:${IMAGE_TAG}").pull()
                    // Run containers on specified ports
                    bat 'docker run -d -p 8082:8080 --name teedy_manual02 teedy2024_manual --fail-never'
                    bat 'docker run -d -p 8083:8080 --name teedy_manual03 teedy2024_manual --fail-never'
                    bat 'docker run -d -p 8084:8080 --name teedy_manual01 teedy2024_manual --fail-never'
                }
            }
        }
    }
    post {
        always {
            // Clean up Docker images and containers to avoid disk space issues
            bat 'docker rm -f teedy_manual01 || true --fail-never'
            bat 'docker rm -f teedy_manual02 || true --fail-never'
            bat 'docker rm -f teedy_manual03 || true --fail-never'
            bat 'docker rmi ${IMAGE_NAME}:${IMAGE_TAG} || true --fail-never'
        }
    }
}
