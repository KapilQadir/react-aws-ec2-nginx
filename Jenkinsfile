pipeline {
    agent any
    tools {
        nodejs 'node18.18'  // Ensure Node.js 18.18 is available in Jenkins
    }
    environment {
        EC2_USER = "ec2-user"
        EC2_HOST = "18.135.69.143"
        PEM_PATH = "/var/lib/jenkins/keys/react-app-training.pem"
        BUILD_DIR = "build"  // The folder where the build artifacts will be generated
        EC2_DEPLOY_DIR = "/var/www/vhosts/frontend"  // Destination folder on EC2 instance
    }

    stages {
        stage('Check Node Version') {
            steps {
                echo '📝 Checking Node.js version in Jenkins'
                sh 'node -v'  // This will print the Node.js version in Jenkins logs
            }
        }

        stage('Build') {
            environment {
                NODE_OPTIONS = "--openssl-legacy-provider"  // Set NODE_OPTIONS for Build stage only
            }
            steps {
                echo '✅ Building the React app in Jenkins'
                sh 'npm install'  // Install dependencies
                sh 'npm run build'  // Build the React app
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying the build to EC2 instance...'
                sh """
                scp -o StrictHostKeyChecking=no -i \$PEM_PATH -r \$BUILD_DIR/* \$EC2_USER@\$EC2_HOST:\$EC2_DEPLOY_DIR
                """
            }
        }

        stage('Restart Nginx on EC2') {
            steps {
                echo '🔄 Restarting Nginx on EC2...'
                sh """
                ssh -o StrictHostKeyChecking=no -i \$PEM_PATH \$EC2_USER@\$EC2_HOST << EOF
                  sudo systemctl restart nginx
                EOF
                """
            }
        }
    }
}
