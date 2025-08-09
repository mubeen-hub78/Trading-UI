pipeline {
    agent any

    environment {
        PM2_HOME = "/var/jenkins_home/.pm2"  // PM2 data inside Jenkins container
    }

    stages {
        stage('Git Checkout') {
            steps {
                git 'https://github.com/betawins/Trading-UI.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm ci || npm install'
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
