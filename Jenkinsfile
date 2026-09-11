
pipeline {
    agent any

    environment {
        NODE_OPTIONS = '--openssl-legacy-provider'
        CI = 'false'
        PATH = "/usr/local/bin:/usr/bin:/bin:${env.PATH}"
    }

    stages {

        stage('Git Checkout') {
            steps {
                git 'https://github.com/Mahesh713-MB/Trading-UI.git'
            }
        }

      stage('Install Dependencies') {
    steps {
        sh '''
            rm -rf node_modules
            npm cache clean --force
            npm install --legacy-peer-deps
        '''
    }
}

        stage('NPM Audit') {
            steps {
                sh 'npm audit || true'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy with PM2') {
            steps {
                sh '''
                    echo "Checking Node and PM2..."
                    node -v
                    npm -v
                    pm2 -v

                    echo "Stopping existing application..."
                    pm2 delete Trading-UI || true

                    echo "Starting React application..."
                    pm2 serve build 3000 --name Trading-UI --spa

                    echo "Saving PM2 configuration..."
                    pm2 save

                    echo "Deployment completed successfully"
                '''
            }
        }
    }
}
