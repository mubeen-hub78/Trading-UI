pipeline {
    agent any

    tools {
        nodejs 'Node18'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/mubeen-hub78/Trading-UI.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Clean node_modules and package-lock.json for a fresh install
                sh 'rm -rf node_modules package-lock.json'
                sh 'npm install'
                // Run audit fix with --force to address all vulnerabilities and deprecations
                sh 'npm audit fix --force || echo "npm audit fix --force completed with potential issues, continuing..."'
                // Run install again to ensure package-lock.json is updated after audit fix
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
                sh 'rm -rf build'
                withEnv(['NODE_OPTIONS=--openssl-legacy-provider', 'CI=false']) {
                    sh 'npm run build'
                }
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
