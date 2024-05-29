pipeline {
    agent any 
    stages {
        stage("Code Clone"){
            steps{
                git url: "https://github.com/SanketShirke/django-todo-app.git", branch: "main"
                echo "Code Cloned"
            }
        }
        stage("Code Build"){
            steps{
                sh "docker build . -t django-app"
                echo "Code Build"
            }
        }
        stage("Push to the docker Hub"){
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubuser")]){
                sh "docker login -u ${env.dockerHubuser} -p ${env.dockerHubPass}"
                sh "docker tag django-app:latest ${env.dockerHubuser}/django-app:latest"
                sh "docker push ${env.dockerHubuser}/django-app:latest"
                }
                echo "Pushing the code on Docker hub"
            }

        }
        stage("Deploy"){
            steps{
                echo "Code Deploy"
                sh "docker compose down && docker compose up -d"
            }
        }
    }
}
