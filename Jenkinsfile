pipeline {
    agent { label 'Dev-Agent' }
    tools {
        jdk 'jdk17'
    }
    environment {


DOCKER_TAG = getVersion()

}
    
    stages{
        stage('code'){
            steps {
                git url: 'https://github.com/Mariam322/ProjetDevops.git', branch: 'main'
            }
        }
      stage('Install Stage') {
            steps {
                withMaven(maven: 'maven4') {
                    dir('etude_de_cas2/spring-boot-server') {  
                        sh 'mvn clean package -DskipTests'
                    }
                }
            }
        }
        stage('Build imageFront'){
            steps {
                sh 'docker build ./etude_de_cas2/angular-14-client/ -t houssem52/angular:${DOCKER_TAG}'
            }
        }
             stage('Build imageBack'){
            steps {
                sh 'docker build ./etude_de_cas2/spring-boot-server/ -t houssem52/spring:${DOCKER_TAG}'
            }
        }
        stage('Login and Push Image'){
            steps {
                echo 'logging in to docker hub and pushing image..'
                withCredentials([usernamePassword(credentialsId:'DockerHub',passwordVariable:'DockerHubPassword', usernameVariable:'DockerHubUsername')]) {
                    sh "docker login -u ${env.DockerHubUsername} -p ${env.DockerHubPassword}"
                    sh "docker push houssem52/angular:${DOCKER_TAG}"
                    sh "docker push houssem52/spring:${DOCKER_TAG}"
                }    
            }
        }
       stage('Deploy') {
    steps {
        script {
            sh """
            export DOCKER_TAG=${DOCKER_TAG}
            docker-compose down
            docker-compose up -d
            """
        }
    }
}
    }
}
def getVersion(){
def version = sh returnStdout: true, script: 'git rev-parse --short HEAD'
return version
}

