pipeline {
    environment{
        dockerHubCredentials = "docker-hub-credentials"
        dockerImage1 = ' '
        dockerHubUsername = "oxlamonsrivne"
        dockerImageTag = "1.0"
        imageName1 = "$dockerHubUsername/wordpress:$dockerImageTag"
        imageName2 = "$dockerHubUsername/db:$dockerImageTag"
        imageName3 = "$dockerHubUsername/webserver:$dockerImageTag"
        imageName4 = "$dockerHubUsername/cerboot:$dockerImageTag"
    }
    agent any
    stages {
        stage('docker-compose up') {
            steps {
                sh 'docker images ls'
                sh 'docker-compose  --env-file ./home/anton/wordpress/.env up -d'
            }
        }
        stage('stop running containers') {
            steps {
                sh 'docker images ls'
                sh 'docker-compose stop'
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
                    dockerImage1 = docker.build(imageName1, ' ')
                    dockerImage2 = docker.build(imageName2, 'db')
                    dockerImage3 = docker.build(imageName3, 'webserver')
                    dockerImage4 = docker.build(imageName4, 'cerboot')
                }
            }

        }
        stage('deploy images'){
            steps{
                script {
                 docker.withRegistry( '', 'dockerHubCredentials') {
                 //sh 'docker push ${dockerHubUsername}/mysql:${dockerImageTag}'
                 dockerImage1.push()
                 dockerImage2.push()
                 dockerImage3.push()
                 dockerImage4.push()
                    }
                }
            }
        }
        stage('remove images'){
            steps{
                sh "docker rmi $imageName1 $imageName2 $imageName3 $imageName4"
           }
        }

    }
}
