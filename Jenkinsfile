@Library("shared") _
pipeline {
    agent any

    stages {
        stage("hello") {
            steps {
                script {
                    hello()
                }
            }
        }

        stage('Clone Repository') {
            steps {
                script {
                    gitCode("https://github.com/ksanoniya/python-flash-app.git", "main")
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t two-tier-app ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhudcer', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker image tag two-tier-app:latest kalashdocker/two-tier-app:latest
                        docker push kalashdocker/two-tier-app:latest
                    """
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sh "docker-compose up -d"  // Change to "docker compose up -d" if using Docker Compose v2
            }
        }
    }
}
