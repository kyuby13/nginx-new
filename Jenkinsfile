env.DOCKER_REGISTRY = 'yubyanime'
env.DOCKER_IMAGE_NAME = 'testing'
pipeline {
    agent any
    stages {
        stage('Git Pull from Github') {
            steps {
                git credentialsId: 'GitHub', url: 'https://github.com/kyuby13/nginx-html.git'
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
        
         stage('Create') {
            steps {
                sh "docker container create --name testing${BUILD_NUMBER} -p 8787:80 $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
            }
        }
          
        stage('Deploy') {
            steps {
                sh "docker container start testing${BUILD_NUMBER}"
            }
        }
         stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }        
    }
}
