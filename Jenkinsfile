def app
pipeline {
    agent any
    stages {
        stage("gradle build") {
            steps {
                echo 'pulling changes...'
                checkout scm
                script {
                    sh "gradle build"
                }
            }
        }
        stage("build docker image") {
            steps {
                echo 'building docker image...'
                script {
                    app = docker.build("kolobokzaebok/java-app")
                }
            }
        }
        stage("test") {
            steps {
                echo 'test stage...'
            }
        }
        stage("push to docker hub") {
            steps {
                echo 'pushing to docker hub...'
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerHub') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }

            }
        }
        stage("pull from docker hub") {
            steps {
                echo 'pulling from docker hub...'
                script {
                    docker.image("kolobokzaebok/java-app:latest").pull()
                }
            }
        }
        stage("deploy") {
            steps {
                echo 'running the java-app from the latest image downloaded from docker hub'
                script {
                    sh "docker run -d -p 7777:8080 kolobokzaebok/java-app:latest"
                }
            }
        }
    }
}
