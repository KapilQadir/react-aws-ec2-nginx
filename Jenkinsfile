pipeline {
    agent any

    environment {
        EC2_USER = "ec2-user"
        EC2_HOST = "18.175.148.176"
        PEM_PATH = "/var/lib/jenkins/keys/react-app-training.pem"
    }

    stages {
        stage('Build') {
            steps {
                echo '✅ Build step - compiling the React app'
            }
        }
        stage('Deploy') {
            steps {
                echo '🚀 Deploying to EC2 instance...'
                sh """
                    ssh -o StrictHostKeyChecking=no -i ${PEM_PATH} ${EC2_USER}@${EC2_HOST} "sudo systemctl restart nginx"
                """
            }
        }
    }
}
