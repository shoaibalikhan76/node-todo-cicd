pipeline {
    agent any
    
    stages {
        stage("code"){
            steps {
                echo "cloning the code"
                git url:"https://github.com/shoaibalikhan76/django-todo-cicd.git", branch: "develop" 
            }    
        }
        stage("build"){
            steps {
                echo "building the code"
                sh "docker build -t notes-app ."
            }
        }
        stage("push to dockerHub"){
            steps {
                echo "pushing the image to docker Hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag notes-app ${env.dockerHubUser}/node-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
            }
        }
        stage("deploy"){
            steps {
                echo "deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
