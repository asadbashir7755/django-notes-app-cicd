// build steps for  notesapp
// another comment
@Library("helloshared") _
pipeline {
    agent { label 'ubuntuagent' }
    

    stages {
        stage("print hello"){
            steps{
                script{
                    hello()
                }
            }
        }
        stage("Clone Code") {
            steps {
                script{
                    clone('https://github.com/asadbashir7755/django-notes-app-cicd.git',"dev")
                }
            }
        }

        stage("Build and Test") {
            steps {
                sh "docker build --rm -t notesapp ."
            }
        }

        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerhublogin", passwordVariable:"dockerHubPass", usernameVariable:"dockerHubUser")]) {
                    sh "docker tag notesapp ${env.dockerHubUser}/notesapp:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/notesapp:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "cd ${env.WORKSPACE} && docker compose down && docker compose up -d"
            }
        }
    }
}
