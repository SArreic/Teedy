// pipeline {
//     agent any
//     environment {
//         DOCKER_CREDENTIALS = 'dockerhub_credentials'
//         IMAGE_NAME = 'sarreic/teedy2024_manual'
//         IMAGE_TAG = 'v1.0'
//     }
//     stages{
//         stage('Package') {
//             steps {
//                 checkout scmGit(branches: [[name: '*/master']], extensions: [],
//                 userRemoteConfigs: [[url: 'https://github.com/SArreic/Teedy.git']])
//                 bat 'mvn -B -DskipTests clean package --fail-never'
//             }
//         }
//         // Building Docker images
//         stage('Building Image') {
//                     steps {
//                         bat "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
//                     }
//                 }
//         // Uploading Docker images into Docker Hub
//         stage('Upload image') {
//                     steps {
//                         script {
//                             // Login to Docker Hub
//                             docker.withRegistry('https://registry.hub.docker.com', DOCKER_CREDENTIALS) {
//                                 // Build and push the Docker image
//                                 def image = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
//                                 image.push()
//                             }
//                         }
//                     }
//                 }
//         //Running Docker container
//         stage('Run containers'){
//             steps{
//                 script{
//                     // Pull the image from Docker Hub
//                     docker.image("${IMAGE_NAME}:${IMAGE_TAG}").pull()
//                     // Run containers on specified ports
//                     bat 'docker run -d -p 8082:8080 --name teedy_manual02 teedy2024_manual'
//                     bat 'docker run -d -p 8083:8080 --name teedy_manual03 teedy2024_manual'
//                     bat 'docker run -d -p 8084:8080 --name teedy_manual01 teedy2024_manual'
//                 }
//             }
//         }
//     }
// }
pipeline {
    agent any
    stages {
        stage('K8s') {
            steps {
                bat 'kubectl set image deployment/h teedy2024_manualeedy=teedy2024_manualeedy:v1.0'
            }
        }
    }
}
