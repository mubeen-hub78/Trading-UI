pipeline {
    agent any

    tools {
        nodejs 'Node18'
    }

    environment {
        PORT = "3000"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/mubeen-hub78/Trading-UI.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    rm -rf node_modules package-lock.json
                    npm install --legacy-peer-deps
                    npm audit fix --force || echo "Audit fix may have failed; continuing"
                '''
            }
        }

        stage('Build Application') {
            steps {
                withEnv(['NODE_OPTIONS=--openssl-legacy-provider', 'CI=false']) {
                    sh 'npm run build'
                }
            }
        }

        stage('Serve Application (Production Mode)') {
            steps {
                sh '''
                    nohup npx serve -s build -l ${PORT} > serve.log 2>&1 &
                    echo "React app started on http://$(curl -s ifconfig.me):${PORT}"
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed. Your app is running.'
            sh 'echo "🔗 Access URL: http://$(curl -s ifconfig.me):${PORT}"'
        }
        failure {
            echo '❌ Pipeline failed. Check logs for more info.'
        }
    }
}
