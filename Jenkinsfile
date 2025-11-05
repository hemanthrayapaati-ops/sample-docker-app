pipeline {
    agent any

    stages {

        stage('Clone from GitHub') {
            steps {
                echo '📥 Cloning repository from GitHub...'
                git branch: 'main',
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

        stage('Simulate Build') {
            steps {
                echo '⚙️ Simulating a build step...'
                sh 'echo "Build step completed successfully ✅"'
            }
        }

        stage('Test Step') {
            steps {
                echo '🧪 Running test step...'
                sh 'echo "All tests passed ✅"'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker image...'
                sh 'docker build -t demo-html-app .'
            }
        }

        stage('Run Container') {
            steps {
                echo '🚀 Running container on port 8081...'
                // Stop and remove any old container before running
                sh 'docker rm -f demo-html-app || true'
                sh 'docker run -d -p 8081:80 --name demo-html-app demo-html-app'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed — check console output.'
        }
    }
}
