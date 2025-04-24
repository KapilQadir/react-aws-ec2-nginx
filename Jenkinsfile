pipeline {
    agent any

    environment {
        EC2_USER = "ec2-user"
        EC2_HOST = "18.175.148.176"
        PEM_PATH = "/var/lib/jenkins/keys/react-app-training.pem"
        REPO_DIR = "/home/ec2-user/react-app" // where your app lives in EC2
        DEPLOY_DIR = "/var/www/vhosts/frontend/build"
    }

    stages {
        stage('Build') {
            steps {
                echo '✅ Build step - compiling the React app'
                // Optional: build locally on Jenkins and scp build files
                // Otherwise, we do it directly on EC2
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying to EC2 instance...'
                sh """
                ssh -o StrictHostKeyChecking=no -i ${PEM_PATH} ${EC2_USER}@${EC2_HOST} << 'EOF'
                    set -e
                    cd ${REPO_DIR}
                    git pull origin main
                    npm install
                    npm run build
                    sudo rm -rf ${DEPLOY_DIR}/*
                    sudo cp -r build/* ${DEPLOY_DIR}/
                    sudo systemctl restart nginx
                EOF
                """
            }
        }
    }
}
