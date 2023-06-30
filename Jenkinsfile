pipeline {

    agent any

    stages {

        stage('Installing Dependencies') {
            steps {
                script {
                    def nodejsTool = tool name: 'node-20-tool', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
                    env.PATH = "${nodejsTool}/bin:${env.PATH}"
                }
                sh 'npm install'
            }
        }

        stage('Build Optimized React Production Files') {
            steps {
                sh 'echo "Build Optimized React Production Files..."'
                sh 'npm run build'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'echo "Build Docker Image..."'
                sh 'docker build -t xaivntaaj/react-docker-jenkins:1.0'
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    def dockerTool = tool name: 'docker-latest-tool', type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
                    env.PATH = "${dockerTool}/bin:${env.PATH}"
                }

                withCredentials([usernamePassword(credentialsId: 'personal-docker-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                }

                sh 'docker push xaivntaaj/react-docker-jenkins:1.0'
            }
        }


    }
}