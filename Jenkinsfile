pipeline{
    agent any;
    stages{
        stage("Code Checkout"){
            steps{
                git url:"https://github.com/Mohammad-Bari/two-tier-flask-app.git", branch: "master"
            }
           
        }
         stage("Build")
            {
                steps{
                    sh 'docker build -t two-tier-flask-app .'
                }
            }
            stage("Push to dockerhub"){
                steps{
                withCredentials([usernamePassword(
                    credentialsId: "dockerhub-credentials",
                    usernameVariable: "DOCKER_USER",
                    passwordVariable: "DOCKER_PASS"
                    )]){
                        
                        sh "docker login -u ${env.DOCKER_USER} -p ${env.DOCKER_PASS}"
                        sh "docker image tag two-tier-flask-app ${env.DOCKER_USER}/two-tier-flask-app"
                        sh "docker push ${env.DOCKER_USER}/two-tier-flask-app:latest"
                        
                    
                    }
                }
            }
            stage("Run docker compose")
            {
                steps{
                    sh 'docker compose up -d'
                }
            }
    }
}
