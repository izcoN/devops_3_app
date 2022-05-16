pipeline {
    agent any

    environment {
     //   scannerHome = tool 'SonarQube'
      //  SONARQUBE_TOKEN = credentials('SONARQUBE_TOKEN')
        DOCKER_HUB_PASSWORD = credentials('DOCKER_HUB_PASSWORD')
    }

    stages {
        stage('Clear running apps') {
            steps {
                sh 'docker rm -f devops_3_app || true'
            }
        }
     /*   stage('Sonarqube analysis frontend') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.login=${SONARQUBE_TOKEN}"
                }
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
       */ 
        stage('Build Docker Image') {
            steps {
                sh "docker build -t devops_3_app -t devops_3_app:latest ."
            }
        }
        stage('Run app') {
            steps {
                sh "docker run -d -p 0.0.0.0:5555:5555 --net=docker_siec --name devops_3_app -t devops_3_app:${BUILD_NUMBER}"
            }
        }
      /* stage('Selenium tests') {
            steps {
                dir('tests/') {
                    sh 'pip3 install -r requirements.txt'
                    sh 'python3 test_app.py'
                }
            }
        }
        */
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
