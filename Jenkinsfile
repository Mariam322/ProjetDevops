pipeline {
    agent { label 'Dev-Agent' }
    tools {
        jdk 'jdk21'
    }
    
    stages{
        stage('code'){
            steps {
                git url: 'https://github.com/Mariam322/ProjetAngularSpring.git', branch: 'main'
            }
        }
      stage('Install Stage') {
            steps {
                withMaven(maven: 'maven4') {
                    dir('BankProject') {  
                        sh 'mvn clean package -DskipTests'
                    }
                }
            }
        }
        stage('Build imageFront'){
            steps {
                sh 'docker build ./BankprojetFront/ -t houssem52/angular-cicd:latest'
            }
        }
             stage('Build imageBack'){
            steps {
                sh 'docker build ./BankProject/ -t houssem52/spring-cicd:latest'
            }
        }
        stage('Login and Push Image'){
            steps {
                echo 'logging in to docker hub and pushing image..'
                withCredentials([usernamePassword(credentialsId:'DockerHub',passwordVariable:'DockerHubPassword', usernameVariable:'DockerHubUsername')]) {
                    sh "docker login -u ${env.DockerHubUsername} -p ${env.DockerHubPassword}"
                    sh "docker push houssem52/node-todo-labapp-cicd:latest"
                }    
            }
        }
        stage('Deploy'){
            steps {
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}

