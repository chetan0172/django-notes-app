@Library("Shared") _
pipeline {
    agent { label "Lolo" }

    stages {
        
        stage("Hello"){
            steps{
                script{
                    hello()
                }
            }
        }

        stage("Code") {
            steps {
                script{
                Clone("https://github.com/chetan0172/django-notes-app.git", "main")
                }
            }
        }
        stage("Build") {
            steps {
                script{
                docker_build("notes-app","latest", "chetan0172")
                }
            }
        }

        stage("Test") {
            steps {
                echo "Testing the application..."
            }
        }

        stage("Push to Dockerhub") {
            steps {
                echo "This is pushing the image to dockerhub"
                withCredentials([usernamePassword(credentialsId: "dockerHubCred", usernameVariable: "dockerHubUser", passwordVariable: "dockerHubPass")]) {
                    sh 'echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin'
                    sh "docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:latest"
                    sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "This is deploying the code"
                sh "docker-compose up -d"
            }
        }
    }
}
