pipeline {
    agent any

    stages {
        stage('Code') {
            steps {
                echo 'This is Developing code'
                git url: "https://github.com/salilgupta332/Django_TODO.git" , branch: "main"
            }
        }
        stage('Build') {
            steps {
                echo 'This is Building code'
                sh  "docker build -t todo ."
            }
        }
         stage("Push to Docker Hub") {
            steps {
                echo 'Pushing Image to Docker hub'
                withCredentials([usernamePassword(credentialsId:"dockerhub" , passwordVariable: "dockerHubPass" , usernameVariable: "dockerHubUser")]){
                    sh "docker tag todo ${env.dockerHubUser}/todo:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push  ${env.dockerHubUser}/todo:latest"
                }
            }
        }
        stage("Deploy") {
            steps {
                echo 'This is Deploying code'
                sh "docker-compose down && docker-compose up -d "
            }
        }
    }
}
