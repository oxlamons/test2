pipeline {
    environment{
        dockerHubCredentials = "docker-hub-credentials"
        dockerImage = ''
       
        dockerHubUsername = "oxlamonsrivne"
        dockerImageTag = "1.0"
        imageName1 = "$dockerHubUsername/wordpress:$dockerImageTag"
        imageName2 = "$dockerHubUsername/db:$dockerImageTag"
        imageName3 = "$dockerHubUsername/webserver:$dockerImageTag"
        imageName4 = "$dockerHubUsername/cerbot:$dockerImageTag"
    }
    agent any
    stages {
        stage('docker-compose up -d') {
            steps {
                sh 'docker ps -a'
                sh 'docker images '
                sh 'docker-compose  up -d'
            }
        }
        stage('stop running containers') {
            steps {
                sh 'docker image ls '
                sh 'docker-compose stop'
                sh 'docker images '
            }
        }
        stage('build images'){
            steps {
                script {
                    sh 'ls -la'
                    //docker.withRegistry('https://hub.docker.com/u/oxlamonsrivne', 'dockerHubCredentials') {
                     //image = docker.image(':bar')
                    // image.pull()
               // sh 'docker tag mysql ${dockerHubUsername}:${dockerImageTag}'
                   //  dockerImage1 = docker.build(imageName1, 'wordpress')
                     //dockerImage2 = docker.build(imageName2, '~/db')
                    //dockerImage3 = docker.build(imageName3, '~/webserver')
                    //dockerImage4 = docker.build(imageName4, '~/cerboot')
                    sh 'docker tag wordpress:5.1.1-fpm-alpine $imageName1'
                    sh 'docker tag mysql:8.0 $imageName2'
                    sh 'docker tag nginx:1.15.12-alpine $imageName3'
                    sh 'docker tag certbot/cerbot $imageName4'
                }
            }

        }
        stage('deploy images'){
            steps{
                script {
                 docker.withRegistry( '', 'dockerHubCredentials') {
                     sh 'docker push oxlamonsrivne/wordpress:$dockerImageTag'
                     sh 'docker push oxlamonsrivne/db:$dockerImageTag'
                     sh 'docker push oxlamonsrivne/webserver:$dockerImageTag'
                     sh 'docker push oxlamonsrivne/cerbot:$dockerImageTag'
                 //dockerImage1.push()
                 //dockerImage2.push()
                // dockerImage3.push()
                 //dockerImage4.push()
                    }
                }
            }
        }
        stage('remove images'){
            steps{
                sh "docker rmi $imageName1  $imageName2 $imageName3 $imageName4"
           }
        }

    }
}
