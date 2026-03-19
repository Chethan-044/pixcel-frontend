pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Chethan-044/pixcel-frontend.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-html-app .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo "Pushing to hub"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                    sh 'docker tag my-html-app ${dockerHubUser}/my-html-app:latest'
                    sh 'echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin'
                    sh 'docker push ${dockerHubUser}/my-html-app:latest'
                }
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker stop my-html-container || true'
                sh 'docker rm my-html-container || true'
                sh 'docker run -d --name my-html-container -p 8081:80 my-html-app'
            }
        }
    }
}
