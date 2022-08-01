env.DOCKER_REGISTRY = 'yubyanime'
env.DOCKER_IMAGE_NAME = 'nginx'
pipeline {
    agent any
    stages {
        stage('Git Pull from Github') {
            steps {
                git credentialsId: 'GitHub', url: 'https://github.com/kyuby13/nginx-new.git'
            }
        }    
       stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER} ."
            }
        }              
        stage('Push Docker Image to Dockerhub') {
            steps {
                sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
            }
        } 
        
        stage('Login') {
            steps {
                 sh """
                    ssh root@13.215.163.182 "docker ps | grep nginx | xargs docker stop; docker pull $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}; docker container create --name nginx${BUILD_NUMBER} -p 8787:80 $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}; docker container start nginx${BUILD_NUMBER}"
                    """
             }
        }
          
        
       
    }
}
