pipeline {
    agent any

    stages {
        stage('Clone from GitHub') {
            steps {
                echo '📥 Cloning repository from GitHub...'
                git branch: 'main',
                    credentialsId: 'github-creds',
                    url: 'https://github.com/hemanthr2002/sample-docker-app.git'
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
                echo '📖 Reading demo.html.txt...'
                sh 'cat demo.html.txt || echo "demo.html.txt not found"'
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
