pipeline {
    agent any
    tools {
        nodejs 'node18.18'
    }
    stage('Check Node Version') {
        sh 'node -v'
    }
    environment {
        EC2_USER = "ec2-user"
        EC2_HOST = "18.135.69.143"
        PEM_PATH = "/var/lib/jenkins/keys/react-app-training.pem"
    }

    stages {
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

