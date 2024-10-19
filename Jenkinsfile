pipeline {
    agent {
        label 'dev'
    }
    stages {
        stage('Clone Code') {
            steps {
                git url: "https://github.com/thepawankrtiwari/django-notes-app.git", branch: "main"
                echo 'Code clone complete...'
            }
        }
        stage('Build & Test') {
            steps {
                sh "docker build . -t notes-app-jenkins:latest"
                echo 'Building docker image...'
            }
        }
        stage("Push to DockerHub") {
            steps { 
                echo "Pushed to DockerHub"
                withCredentials(
                    [usernamePassword(
                        credentialsId: 'dockerCreds', 
                        passwordVariable: 'dockerHubPass',
                        usernameVariable: 'dockerHubUser' 
                        )
                    ]
                ){
                sh "docker image tag notes-app-jenkins:latest ${env.dockerHubUser}/notes-app-jenkins:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/notes-app-jenkins:latest"
                }
            }       
        }

        stage('Deploy') {
            steps {
                sh "docker compose down && docker compose up -d"
                echo 'Docker run command is working and running...'
            }
        }
    }
}
