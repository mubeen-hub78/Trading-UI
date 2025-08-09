pipeline {
    agent any

    environment {
        PM2_HOME = "/var/jenkins_home/.pm2"
    }

    stages {
        stage('Git Checkout') {
            steps {
                git 'https://github.com/mubeen-hub78/Trading-UI.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Start with PM2') {
            steps {
                sh '''
                    mkdir -p $PM2_HOME
                    pm2 delete Trading-UI || true
                    pm2 --name Trading-UI start npm -- start
                    pm2 save
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution finished.'
        }
    }
}
