pipeline {
    agent any

    environment{
		    DOCKER_IMAGE_OWNER = 'docdker'
		    DOCKER_IMAGE_TAG = 'latest'
		    DOCKER_TOKEN = credentials('dockerhub')
    }
    stages {
        stage('clone from SCM') {
            steps {
                sh '''
                rm -rf project-parking
                git clone https://github.com/evollivia/project-parking.git
                '''
            }
        }
        stage('Docer Image Builging') {
            steps {
                sh '''
		cd project-parking
                docker build -t ${DOCKER_IMAGE_OWNER}/msa-frontend-nodejs:${DOCKER_IMAGE_TAG} -f nodejs-Dockerfile ./msa-frontend
                docker build -t ${DOCKER_IMAGE_OWNER}/msa-frontend-nginx:${DOCKER_IMAGE_TAG} -f nginx-Dockerfile ./msa-frontend
                docker build -t ${DOCKER_IMAGE_OWNER}/msa-register-service:${DOCKER_IMAGE_TAG} ./msa-register-service
                docker build -t ${DOCKER_IMAGE_OWNER}/msa-payment-service:${DOCKER_IMAGE_TAG} ./msa-payment-service
                docker build -t ${DOCKER_IMAGE_OWNER}/msa-parking-service:${DOCKER_IMAGE_TAG} ./msa-parking-service
                docker build -t ${DOCKER_IMAGE_OWNER}/msa-statistics-service:${DOCKER_IMAGE_TAG} ./msa-statistics-service
                '''
            }
        }
        stage('Docer Login') {
            steps {
                sh '''
                echo ${DOCKER_TOKEN_PSW} | docker login -u ${DOCKER_TOKEN_USR} --password-stdin
                '''
            }
        }
        stage('Docer Image Pushing') {
            steps {
                sh '''
                docker push ${DOCKER_IMAGE_OWNER}/msa-frontend:${DOCKER_IMAGE_TAG}
                docker push ${DOCKER_IMAGE_OWNER}/msa-register-service:${DOCKER_IMAGE_TAG}
                docker push ${DOCKER_IMAGE_OWNER}/msa-payment-service:${DOCKER_IMAGE_TAG}
                docker push ${DOCKER_IMAGE_OWNER}/msa-parking-service:${DOCKER_IMAGE_TAG}
                docker push ${DOCKER_IMAGE_OWNER}/msa-statistics-service:${DOCKER_IMAGE_TAG}
                '''
            }
        }
        stage('Docer Logout') {
            steps {
                sh '''
                docker logout
                '''
            }
        }
    }
}
