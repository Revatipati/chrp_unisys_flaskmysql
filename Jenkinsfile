pipeline {
    agent any

    stages {
        stage('fetching github code') {
            steps {
                // using echo to print message 
                echo 'we are now fetching github code in below'
                // using git to download git repo 
                git branch: 'master', url: 'https://github.com/redashu/ashu_unisys_flaskMysql.git'
                // using sh command to run some commands
                sh 'ls '
            }
        }
        // verify docker and trivy 
        stage('checking docker and compose status') {
            steps {
                echo 'checking docker status'
                sh 'docker version'
                echo 'checking docker compose status'
                sh 'docker-compose version'
                
            }
            
        }
        // using compsoe and curl 
        stage('compsoe for build and curl for app status test') {
            steps {
                echo 'running docker compose'
                sh 'docker-compose down'
                sh 'docker-compose up -d --build'
                sh 'docker-compose ps'
                sh 'curl -f http://localhost:3001'
            }
            
        }
        // using docker pipeline to build and push 
        stage('building image and pushing it') {
            steps {
                echo 'using docker pipeline plugin to build and push image'
                script {
                    def imageName = "dockerashu/flaskday4"
                    def imageTag  = "appversion$BUILD_NUMBER"
                    def ashuCred = "480087f2-27c4-4acf-b208-19b1e0b0cf57"
                    // building image 
                    docker.build(imageName + ":" + imageTag , " -f Dockerfile .")
                    // pushing image 
                    docker.withRegistry('https://registry.hub.docker.com',ashuCred){
                        docker.image(imageName + ":" + imageTag).push()
                    }
                }
            }
            
        }
    }
}