pipeline{
    agent { label "dev-server" }
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/mujawarsaad/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Build and Test"){
            steps{
                sh "docker build -t node-app ."
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass")]){
                    sh 'echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin'
                    sh 'docker image tag node-app:latest $dockerHubUser/node-app:latest'
                    sh 'docker push $dockerHubUser/node-app:latest'
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose down && docker compose up -d --build"
            }
        }
    }
}
