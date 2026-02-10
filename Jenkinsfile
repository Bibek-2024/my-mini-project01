pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "bibekdec2022/my-mini-projects"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Bibek-2024/my-mini-project01.git', credentialsId: 'github-token'
            }
        }
        stage('Build & Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker build -t $DOCKER_IMAGE:${BUILD_NUMBER} .'
                    sh 'docker push $DOCKER_IMAGE:${BUILD_NUMBER}'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl set image deployment/web web=$DOCKER_IMAGE:${BUILD_NUMBER} --record'
            }
        }
    }
}
