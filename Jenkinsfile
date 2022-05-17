pipeline {
    agent anydevops_3_app

    environment {
        DOCKER_HUB_PASSWORD = credentials('DOCKER_HUB_PASSWORD')
        
    }

    stages {
        stage('Clear running apps') {
            steps {
                sh 'docker rm -f devops_3_app || true'
            }
        }
    
        stage('Build Docker Image') {
            steps {
                sh "docker build -t devops_3_app:${BUILD_NUMBER} -t devops_3_app:latest ."
            }
        }
        stage('Run app') {
            steps {
                sh "docker run -d -p 127.0.0.1:5555:5555 --net=jenkins_default --name devops_3_app -t devops_3_app:${BUILD_NUMBER}"
            }
        }
        stage('Upload Docker Image to Docker Hub') {
            steps {
                sh "docker login -u damiantkh -p ${DOCKER_HUB_PASSWORD}"
                sh "docker tag devops_3_app:${BUILD_NUMBER} damiantkh/devops_3_app:${BUILD_NUMBER}"
                sh 'docker tag devops_3_app:latest damiantkh/devops_3_app:latest'
                sh "docker push damiantkh/devops_3_app:${BUILD_NUMBER}"
                sh 'docker push damiantkh/devops_3_app:latest'
            }
        }
    }
}