pipeline {
    agent { label 'dev' }

    stages {
        stage('Code Clone Stage') {
            steps {
                git url: 'https://github.com/kishorpatil2107/two-tier-flask-app.git', branch: 'master'
                echo 'Code clone done...'
            }
        }

        stage('Code Build Stage') {
            steps {
                sh 'docker build -t two-tier-flask-app:latest .'
                echo 'Code build done...'
            }
        }
        
        stage('Code Test Stage') {
            steps {
                echo 'Code test done...'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerHuCreds",
                    usernameVariable: "dockerHubUsr",
                    passwordVariable: "dockerHubPass"
                )]) {
                    sh "docker login -u $dockerHubUsr -p $dockerHubPass"
                    sh "docker tag two-tier-flask-app $dockerHubUsr/two-tier-flask-app:latest"
                    sh "docker push $dockerHubUsr/two-tier-flask-app:latest"
                }
            }
        }
        

        stage('Code Deploy Stage') {
            steps {
                sh 'docker compose pull'
                sh 'docker compose up -d'
                echo 'Code deploy done...'
            }
        }
    }
}

// @Library("Shared") _
// pipeline{
    
//     agent { label "dev"};
    
//     stages{
//         stage("Code Clone"){
//             steps{
//                script{
//                    clone("https://github.com/kishorpatil2107/two-tier-flask-app.git", "master")
//                }
//             }
//         }
//         stage("Trivy File System Scan"){
//             steps{
//                 script{
//                     trivy_fs()
//                 }
//             }
//         }
//         stage("Build"){
//             steps{
//                 sh "docker build -t two-tier-flask-app ."
//             }
            
//         }
//         stage("Test"){
//             steps{
//                 echo "Developer / Tester tests likh ke dega..."
//             }
            
//         }
//         stage("Push to Docker Hub"){
//             steps{
//                 script{
//                     docker_push("dockerHubCreds","two-tier-flask-app")
//                 }  
//             }
//         }
//         stage("Deploy"){
//             steps{
//                 sh "docker compose up -d --build flask-app"
//             }
//         }
//     }

post{
        success{
            script{
                emailext from: 'kishorpatil2107@gmail.com',
                to: 'kishorpatil2107@gmail.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            }
        }
        failure{
            script{
                emailext from: 'kishorpatil2107@gmail.com',
                to: 'kishorpatil2107@gmail.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            }
        }
    }
}
