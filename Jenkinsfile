pipeline {
    agent any
    
    stages{
        stage("code clone"){
            steps{
                echo "code cloned"
                git url:"https://github.com/oppo-A5/django-notes-app.git", branch: "main"
            }
        }
        stage("code build"){
            steps{
                echo "code has built"
                sh "docker build -t note-app ."
            }
        }
        stage("push to dockerHub"){
            steps{
                echo "code push to dockerhub"
                withCredentials([usernamePassword(credentialsId: 'DockerHub', passwordVariable: 'DockerHubPass', usernameVariable: 'DockerHubUser')]){
                sh "docker tag note-app ${env.DockerHubUser}/note-app:latest"    
                sh "docker login -u ${env.DockerHubUser} -p ${env.DockerHubPass}"
                sh "docker push ${env.DockerHubUser}/note-app:latest"
                }
            }
        }
        stage("code deploy"){
            steps{
                echo "code deployed on container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
