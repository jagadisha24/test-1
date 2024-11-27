pipeline {
    agent any
    tools {
        jdk 'java-11'
        maven 'maven'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/jagadisha24/test-1.git'
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Build and Tag Docker File') {
            steps {
                sh '''
                    docker build -t jagadishasiddaiah/puneethrajkumar:1 .
                '''
            }
        }
        stage('Docker Image Scan') {
            steps {
                sh '''
                    trivy image --format table \
                    -o trivy-image-report.html jagadishasiddaiah/puneethrajkumar:1
                '''
            }
        }
        stage('Containerization') {
            steps {
                sh '''
                    docker stop c1 || true
                    docker rm c1 || true
                    docker run -it -d --name c1 -p 9001:8080 jagadishasiddaiah/puneethrajkumar:1
                '''
            }
        }
        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    }
                }
            }
        }
        stage('Pushing image to repository') {
    steps {
        sh 'docker push jagadishasiddaiah/puneethrajkumar:1'
    }
}

    }
}
