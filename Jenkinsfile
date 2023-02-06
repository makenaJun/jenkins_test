def app

pipeline {
    agent any
    environment {
        ENV_TYPE = "STAGING"
        PORT = 4000
        REGISTRY_HOSTNAME = "makenajun"
        PROJECT = "jenkins-test"
        REGISTRY = "registry.hub.docker.com"
        DEPLOYMENT_NAME = "jenkins-test-deployment"
        IMAGE_NAME = "${env.BUILD_ID}_${env.ENV_TYPE}_${env.GIT_COMMIT}"
        DOCKER_BUILD_NAME = "${env.REGISTRY_HOSTNAME}/${env.PROJECT}:${env.IMAGE_NAME}"
    }

    stages {
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }
        stage('Build docker image') {
            steps {
                echo "Build image started..."
                    script {
                        app = docker.build("${env.DOCKER_BUILD_NAME}")
                    }
                echo "Build image finished..."
            }
        }
        stage('Push docker image') {
             steps {
                 echo "Push image started..."
                     script {
                        docker.withRegistry("https://${env.REGISTRY}", 'docker-hub') {
                            app.push("${env.IMAGE_NAME}")
                        }
                     }
                 echo "Push image finished..."
             }
       }
       stage('Delete image local') {
             steps {
                 script {
                    sh "docker rmi ${env.DOCKER_BUILD_NAME}"
                    sh "docker rmi ${env.REGISTRY}/${env.DOCKER_BUILD_NAME}"
                 }
             }
        }
    }
}