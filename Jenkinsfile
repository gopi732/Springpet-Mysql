// Declarative pipeline
pipeline {
    agent any
    
    environment {
        DOCKER_HUB_REPO = "saigopi123456/springpet
        DOCKER_HUB_REPO1 = "saigopi123456/mysql"
        CONTAINER_NAME = "spring-mysql-container"
        http_proxy = 'http://127.0.0.1:3128/'
        https_proxy = 'http://127.0.0.1:3128/'
        ftp_proxy = 'http://127.0.0.1:3128/'
        socks_proxy = 'socks://127.0.0.1:3128/'
    }

    stages {
        stage ('Clean up') {
            steps {
                sh 'docker stop $(docker ps -a -q) || true && docker rm $(docker ps -a -q) || true && docker rmi -f $(docker images -a -q) || true'  
            }
        }
        stage ('Build Docker Image') {
            steps {
                sh 'docker-compose build '       
            }
        }
        stage ('Rename Docker Image') {
            steps {
                sh 'docker tag spring-pet-mysql_database  $DOCKER_HUB_REPO && docker tag spring-pet-mysql_spring $DOCKER_HUB_REPO1'       
            }
        }
        stage ('create container'){
            steps {
                    sh 'docker-compose up -d && docker ps'
            }
        }
        stage ('Container Testing '){
            steps {
                    sh 'wget localhost:8089'
            }
        }
        
        stage('Docker login and Push image to DockerHub'){
            steps{
                   withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhub')]) {
                   sh 'docker login -u saigopi123456 -p ${dockerhub} && docker push $DOCKER_HUB_REPO:$BUILD_NUMBER && docker push $DOCKER_HUB_REPO1:$BUILD_NUMBER '
}
            }
        }
    }
}
