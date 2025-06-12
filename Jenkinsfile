pipeline {
    agent any

    tools {
        nodejs 'Node18'  // Use the Node.js version configured in Jenkins
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/mubeen-hub78/Trading-UI.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Code Linting (Optional)') {
            steps {
                sh 'npm run lint || echo "Lint failed, continuing..."'
            }
        }

        stage('Build Application') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Test (Optional)') {
            steps {
                sh 'npm test || echo "Tests failed, continuing..."'
            }
        }

        stage('Start Application (Dev Mode)') {
            steps {
                sh 'npm start &'
            }
        }
    }

    post {
        success {
            echo '✅ Node.js App pipeline executed successfully.'
        }
        failure {
            echo '❌ Pipeline failed. Check the console output for issues.'
        }
    }
}
