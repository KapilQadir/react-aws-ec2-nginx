pipeline {
    agent any
    tools {
        nodejs 'node18.18'
    }
    environment {
        EC2_USER = "ec2-user"
        EC2_HOST = "18.135.69.143"
        PEM_PATH = "/var/lib/jenkins/keys/react-app-training.pem"
    }

    stages {
        stage('Check Node Version') {
            steps {
                echo '📝 Checking Node.js version'
                sh 'node -v'  // This will print the Node.js version in Jenkins logs
            }
        }
        stage('Build') {
            steps {
                echo '✅ Build step - compiling the React app'
                // Optionally run local npm build here if needed
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying to EC2 instance...'
                sh """
                ssh -o StrictHostKeyChecking=no -i \$PEM_PATH \$EC2_USER@\$EC2_HOST << EOF
                  cd ~/react-app
                  git pull origin main
                  nvm use 18.18.0
                  node -v
                  npm install
                  npm run build
                  sudo systemctl restart nginx
                EOF
                """
            }
        }
    }
}

