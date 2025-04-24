pipeline {
    agent any  // Runs the pipeline on any available agent

    environment {
        // Optional: define environment variables here
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                git branch: 'main', url: 'https://github.com/KapilQadir/react-aws-ec2-nginx.git'
            }
        }

        stage('Build') {
            steps {
                // Run build steps, for example, building a React app
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                // Run tests
                sh 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                // Deploy the app (for example, to EC2)
                sh 'scp -i /path/to/key.pem -r build/* ec2-user@your-ec2-ip:/path/to/deploy/directory/'
                sh 'ssh -i /path/to/key.pem ec2-user@your-ec2-ip "sudo systemctl restart nginx"'
            }
        }
    }

    post {
        always {
            // Actions that always run after the pipeline completes
            echo 'Pipeline completed.'
        }
        success {
            // Actions to take if the pipeline succeeds
            echo 'Pipeline succeeded.'
        }
        failure {
            // Actions to take if the pipeline fails
            echo 'Pipeline failed.'
        }
    }
}
