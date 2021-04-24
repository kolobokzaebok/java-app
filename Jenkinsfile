def app
pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDS = credentials('dockerHub')
    }

    stages {
        stage("fetch changes from github") {
            steps {
                echo 'scan github for code changes'
                checkout scm
            }
        }
        stage("build") {
            steps {
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
                    docker.withRegistry('https://registry.hub.docker.com', ${env.DOCKER_HUB_CREDS}) {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }

            }
        }
//        stage("cleanup") {
//            steps {
//                echo 'running cleanup...'
//                sh "docker rmi kolobokzaebok/java-app:${env.BUILD_NUMBER}"
//            }
//        }
    }
}
