pipeline {
    agent any

    environment {
        IMAGE_NAME = "hemanthr2002/demo-html-app"
    }

    stages {
        stage('Clone from GitHub') {
            steps {
                echo "📥 Cloning ${env.BRANCH_NAME} branch from GitHub..."
                git branch: "${env.BRANCH_NAME}",
                    credentialsId: 'github-creds',
                    url: 'https://github.com/hemanthrayapaati-ops/sample-docker-app.git'
            }
        }

        stage('Check Workspace') {
            steps {
                echo '📂 Checking project folder...'
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
                echo "🐳 Building Docker image for ${env.BRANCH_NAME} branch..."
                sh 'docker build -t ${IMAGE_NAME}:${env.BRANCH_NAME} .'
            }
        }

        stage('Run Container') {
            steps {
                echo "🚀 Running container for ${env.BRANCH_NAME}..."
                sh 'docker rm -f demo-html-app || true'
                sh 'docker run -d -p 8081:80 --name demo-html-app ${IMAGE_NAME}:${env.BRANCH_NAME}'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo "📦 Pushing Docker image for ${env.BRANCH_NAME} to Docker Hub..."
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                    sh 'docker push ${IMAGE_NAME}:${env.BRANCH_NAME}'
                }
            }
        }
    }

    post {
        success {
            echo "✅ ${env.BRANCH_NAME} build completed and image pushed successfully!"
        }
        failure {
            echo "❌ ${env.BRANCH_NAME} pipeline failed — check console output."
        }
    }
}
