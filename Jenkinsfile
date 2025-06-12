pipeline {
    agent any

    tools {
        nodejs 'Node18'
    }

    environment {
        NODE_ENV = 'production'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/mubeen-hub78/Trading-UI.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'rm -rf node_modules package-lock.json'
                sh 'npm cache clean --force'
                sh 'npm install --legacy-peer-deps'
            }
        }

        stage('Build Application') {
            steps {
                sh 'rm -rf build'
                withEnv(['NODE_OPTIONS=--openssl-legacy-provider', 'CI=false']) {
                    sh 'npm run build'
                }
            }
        }

        stage('Serve Application (Production Mode)') {
            steps {
                // Install serve only once
                sh 'npm install -g serve'
                // Start serve in background (port 3000)
                sh 'nohup serve -s build -l 3000 > serve.log 2>&1 &'
            }
        }
    }

    post {
        success {
            echo '✅ App deployed successfully. Access it at:'
            sh 'curl ifconfig.me && echo ":3000"'
        }
        failure {
            echo '❌ Pipeline failed. Check logs.'
        }
    }
}
