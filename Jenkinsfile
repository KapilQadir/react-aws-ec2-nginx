pipeline {
    agent any

    tools {
        nodejs 'node18.18'  // Ensure Node.js 18.18 is available in Jenkins
    }

    environment {
        EC2_USER = "ec2-user"
        EC2_HOST = "18.135.69.143"
        BUILD_DIR = "build"
        EC2_DEPLOY_DIR = "/var/www/vhosts/frontend"
    }

    stages {
        stage('Check Node Version') {
            steps {
                echo '📝 Checking Node.js version in Jenkins'
                sh 'node -v'
            }
        }

        stage('Build') {
            environment {
                NODE_OPTIONS = "--openssl-legacy-provider"
            }
            steps {
                echo '✅ Building the React app in Jenkins'
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Deploy to EC2') {
            steps {
                echo '🚀 Deploying the build to EC2 instance using SSH credentials'
                sshagent(['react-app-training-ssh']) {
                    sh """
                        scp -o StrictHostKeyChecking=no -r ${BUILD_DIR}/* ec2-user@${EC2_HOST}:${EC2_DEPLOY_DIR}
                    """
                }
            }
        }

       stage('Restart Nginx') {
            steps {
                echo '🔄 Restarting Nginx on EC2 instance...'
                sshagent(['react-app-training-ssh']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ec2-user@${EC2_HOST} 'sudo systemctl restart nginx'
                    """
                }
            }
        }
    }
}
