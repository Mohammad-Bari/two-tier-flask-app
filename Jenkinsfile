@Library ("Shared") _
pipeline {
    agent { label 'dev' }
    stages {
        stage('Code Checkout') {
            steps {
                script{
                    clone("https://github.com/Mohammad-Bari/two-tier-flask-app", "master")
                }
            }
        }
        stage("Trivy file system scan"){
            steps{
                script{
                    trivy()
                }
            }
        }
        stage('Build')
            {
                steps {
                    sh 'docker build -t two-tier-flask-app .'
                }
            }
            stage('Push to dockerhub') {
                steps {
                        script{
                            docker_push("dockerhub-credentials", "two-tier-flask-app")
                        }
                }
            }
            stage('Run docker compose')
            {
                steps {
                    sh 'docker compose up -d'
                }
            }
    }
    post{
        success {
            emailext(
                subject: "Build Successful",
                body: "Good News: Build is successfull!",
                to: "nooraliulbari@gmail.com"
            )
        }
        failure {
            emailext (
            subject: "Build Failed",
            body: "Bad News: Your build is failed",
            to: "nooraliulbari@gmail.com"
            )
        }
    }
}
