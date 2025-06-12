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
                    npm audit fix || echo "Audit fix skipped errors"
                    # Reinstall react-scripts in case audit fix removed it
                    npm install react-scripts@5.0.1 --save-dev
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

        stage('Serve Application') {
            steps {
                sh '''
                    nohup npx serve -s build -l $PORT > serve.log 2>&1 &
                    sleep 5
                    echo "✅ App is running at: http://$(curl -s ifconfig.me):$PORT"
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline finished successfully.'
            sh 'echo "Access App: http://$(curl -s ifconfig.me):$PORT"'
        }
        failure {
            echo '❌ Pipeline failed. Check logs above.'
        }
    }
}
