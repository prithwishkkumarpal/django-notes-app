pipeline {
    agent any
    
    stages {
        stage("code"){
            steps{
                echo "Cloning the code"
                git url: "https://github.com/shrihariharidass/django-notes-app.git", branch: "main"
            }
        }
        stage("Build"){
             steps{
                echo "Building the code"
                sh "docker build -t my-notes-app ."
            }
        }
        stage("Push to Dockerhub"){
             steps{
                echo "Pushing the image to dockerhub"
                withCredentials([usernamePassword(credentialsId:"dockerHub", passwordVariable:"dockerHubPass", usernameVariable:"dockerHubUser")]){
                sh "docker tag my-notes-app ${env.dockerHubUser}/my-notes-app:V1"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-notes-app:V1"
                }
            }
        }
        stage("Deploy"){
             steps{
                echo "Deploying the cotainer"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
