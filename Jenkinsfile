pipeline {
    agent { label 'ubuntuagent' }

    stages {
        stage("Clone Code") {
            steps {
                git url: "https://github.com/asadbashir7755/django-notes-app-cicd.git", branch: "dev"
            }
        }

        stage("Build and Test") {
            steps {
                sh "docker build --rm -t notesapp ."
            }
        }

        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerHub", passwordVariable:"dockerHubPass", usernameVariable:"dockerHubUser")]) {
                    sh "docker tag notesapp ${env.dockerHubUser}/notesapp:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/notesapp:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "cd ${env.WORKSPACE} && docker-compose down && docker-compose up -d"
            }
        }
    }
}
