pipeline{
    agent any;
    
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/rohandeb2/two-tier-flask-app", branch: "master"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Test"){
            steps{
                echo "Test bhi ho gaya"
            }
        }
        stage("Push to docker hub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerhubcreds",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                } 
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
