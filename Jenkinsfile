pipeline {
    agent any
    environment{
        DOCKERHUB_CREDENCIALS = credentials ('dockerhub')
        RepoDockerHub = 'zdenkoo98'
        NameContainer = 'pokedex-flask'
    }

    stages {
        stage('Build'){
            steps{
                sh "docker build -t ${env.RepoDockerHub}/${env.NameContainer}:${env.BUILD_NUMBER} ."
            }
        }

        stage('Login to Dockerhub'){
            steps{
                sh "echo $DOCKERHUB_CREDENCIALS_PSW | docker login -u $DOCKERHUB_CREDENCIALS_USR --password-stdin "
            }
        }

        stage('Push image to Dockerhub'){
            steps{
                sh "docker push ${env.RepoDockerHub}/${env.NameContainer}:${env.BUILD_NUMBER} "
            }
        }

        stage('Deploy container'){
            steps{
                sh "if [ 'docker stop ${env.NameContainer}' ] ; then docker rm -f ${env.NameContainer} && docker run -d --name ${env.NameContainer} -p 5000:5000 ${env.RepoDockerHub}/${env.NameContainer}:${env.BUILD_NUMBER} ; else docker run -d --name pokedex-flask -p 5000:5000 ${env.RepoDockerHub}/${env.NameContainer}:${env.BUILD_NUMBER} ; fi"
            }
        }
        
        stage('Docker logout'){
            steps{
                sh "docker logout"
            }
        }
    }
}