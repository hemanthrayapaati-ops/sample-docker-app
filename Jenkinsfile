pipeline {
    agent any

    environment {
        IMAGE_NAME = "hemanthr2002/demo-html-app"  // ✅ your actual Docker Hub repo
    }
stages {
        stage('Clone Repository') {
            steps {
                echo "📥 Cloning ${env.BRANCH_NAME} branch from GitHub..."
                git branch: "${env.BRANCH_NAME}",
                    credentialsId: 'github-creds',
                    url: 'https://github.com/hemanthrayapaati-ops/sample-docker-app.git'
            }
        }

        stage('Check Workspace') {
            steps {
                echo '📂 Checking workspace contents...'
                sh 'pwd'
                sh 'ls -l'
            }
        }
        stage('Read File') {
            steps {
                echo '📖 Reading demo.html...'
                sh 'cat demo.html || echo "demo.html not found"'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker image...'
                sh 'docker build -t ${IMAGE_NAME}:latest .'
            }
        }

        stage('Run Container') {
            steps {
                echo '🚀 Running container on port 8081...'
                sh 'docker rm -f demo-html-app || true'
                sh 'docker run -d -p 8081:80 --name demo-html-app ${IMAGE_NAME}:latest'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo '📦 Pushing Docker image to Docker Hub...'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                    sh 'docker push ${IMAGE_NAME}:latest'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully and image pushed to Docker Hub!'
        }
        failure {
            echo '❌ Pipeline failed — check console output.'
        }
    }
}
