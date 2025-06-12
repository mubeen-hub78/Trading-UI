pipeline {
    agent any

    tools {
        nodejs 'Node18'
        dockerTool 'Docker'  // Make sure Docker is installed and added in Jenkins global tools
    }

    environment {
        IMAGE_NAME = "bank-ui"
        CONTAINER_NAME = "bank-ui-container"
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

        stage('Build Docker Image') {
            steps {
                sh '''
                    echo "FROM nginx:alpine" > Dockerfile
                    echo "COPY build /usr/share/nginx/html" >> Dockerfile
                    docker build -t $IMAGE_NAME .
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                    docker rm -f $CONTAINER_NAME || true
                    docker run -d --name $CONTAINER_NAME -p $PORT:80 $IMAGE_NAME
                '''
            }
        }

        stage('Print URL') {
            steps {
                sh 'echo "✅ App running at: http://$(curl -s ifconfig.me):$PORT"'
            }
        }
    }

    post {
        success {
            echo '✅ Deployment complete. Access your app via public URL.'
        }
        failure {
            echo '❌ Deployment failed. See logs.'
        }
    }
}
