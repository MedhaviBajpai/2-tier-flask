pipeline {
    agent any

    stages {
        stage("CODE") {
            steps {
                git url: "https://github.com/suyash700/two-tier-flask-app.git", branch: "master"
            }
        }

        stage("BUILD") {
            steps {
               sh "docker build --no-cache -t my-flask-app:latest ."

            }
        }

        stage("TEST") {
            steps {
                echo "testing"
            }
        }

        stage("PUSH TO DOCKERHUB") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerhubId",
                    usernameVariable: "dockerhubuser",
                    passwordVariable: "dockerHubpass"
                )]) {
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerHubpass}"
                    sh "docker image tag my-flask-app:latest ${env.dockerhubuser}/two-tier-app:latest"
                    sh "docker push ${env.dockerhubuser}/two-tier-app:latest"
                }
            }
        }

        stage("DEPLOY") {
            steps {
                sh "docker-compose up -d"
            }
        }
    }
}
