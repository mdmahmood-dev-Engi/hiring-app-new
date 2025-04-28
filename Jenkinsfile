pipeline {
    agent any
    environment {
        // Define your environment variables here
        DOCKERHUB_CREDENTIALS = credentials('docker-id') // Jenkins credentials ID for DockerHub
        DOCKER_IMAGE = 'uddinmdmahmood/hiring-app' // Replace with your DockerHub image name
        VERSION = "${env.BUILD_ID}" // Using build number as version
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/mdmahmood-dev-Engi/hiring-app-new.git' // Replace with your repo URL
            }
        }
        stage('Build with Maven') {
            steps {
                sh 'mvn clean package' // This should generate your WAR file
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${env.DOCKER_IMAGE}:${env.VERSION}")
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-id') {
                        docker.image("${env.DOCKER_IMAGE}:${env.VERSION}").push()
                        // Optionally push as latest
                        docker.image("${env.DOCKER_IMAGE}:${env.VERSION}").push('latest')
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
                       
