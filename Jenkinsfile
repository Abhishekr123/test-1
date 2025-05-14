pipeline {
    agent any
    tools{
        jdk 'java-17'
        maven 'maven'
    }
    stages{
        stage('git checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/Abhishekr123/test-1.git'
            }
        }
        stage('compile'){
            steps{
                sh "mvn compile"
            }
        }
        stage('Build'){
            steps{
                sh "mvn clean install" 
            }
        }
        stage('Build and Tag Docker file'){
            steps{
                sh "docker build -t abhishek12346/project:1 ."
            }
        }
        stage('Docker image scan'){
            steps{
                 sh "trivy image --format table -o trivy-image-report.html abhishek12346/project:1"
            }
        }

        stage('Containersation'){
            steps{
                sh '''
                    docker run -it -d --name c1 -p 9002:8080 abhishek12346/project:1
                '''
            }
        }

        stage('Login to Docker Hub') {
                    steps {
                        script {
                            withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                                sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                            }
                        }
                    }
        }
        stage('Pushing image to repository'){
            steps{
                sh 'docker push abhishek12346/project:1'
            }
        }
    }
}