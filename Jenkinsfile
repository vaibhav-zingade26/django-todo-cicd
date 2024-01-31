pipeline {
    agent {label "dev-server"}

    stages {
        stage('code') {
            steps {
                git url: "https://github.com/vaibhav-zingade26/django-todo-cicd" , branch: "develop"
                echo 'code cloned successfully'
            }
        }
         stage('build and test') {
            steps {
                sh "docker build -t django-todo-cicd ."
                echo 'build and test'
            }
        }
         stage('push image on docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerHub",usernameVariable:"dockerHubUser",passwordVariable:"dockerHubPass")]){
                sh "docker tag django-todo-cicd ${env.dockerHubUser}/django-todo-cicd:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/django-todo-cicd:latest"
                echo 'pushed image'
            }
            }
        }
         stage('security check') {
            steps {
                echo 'security check'
            }
        }
         stage('deployment') {
            steps {
                sh "docker-compose down && docker-compose up -d"
                echo 'deployment of an code'
            }
        }
    }
}
