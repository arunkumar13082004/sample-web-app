pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/arunkumar13082004/sample-web-app.git', branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t sample-web-app:latest .'
            }
        }
        stage('Tag Image') {
            steps {
                sh 'docker tag sample-web-app:latest arunkumar13082004/sample-web-app:latest'
            }
        }
        stage('Push Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push arunkumar13082004/sample-web-app:latest'
                }
            }
        }
        stage('Pull Image from Docker Hub') {
            steps {
                sh 'docker pull arunkumar13082004/sample-web-app:latest'
            }
        }
        stage('Run Docker Container') {
            steps {
                sh '''
                docker rm -f sample-web-app || true
                docker run -d -p 80:80 --name sample-web-app arunkumar13082004/sample-web-app:latest
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
