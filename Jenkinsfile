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
                // Optional: build locally on Jenkins and scp build files
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying to EC2 instance...'
                sh """
                    ssh -o StrictHostKeyChecking=no -i \$PEM_PATH \$EC2_USER@\$EC2_HOST << 'EOF'
                        REPO_URL="https://github.com/KapilQadir/react-aws-ec2-nginx.git"
                        APP_DIR="/home/ec2-user/react-app"
                        NGINX_DIR="/var/www/vhosts/frontend"

                        # Clone if not exists
                        if [ ! -d "\$APP_DIR" ]; then
                            echo "📥 Cloning repository..."
                            git clone \$REPO_URL \$APP_DIR
                        fi

                        cd \$APP_DIR
                        echo "📡 Pulling latest changes..."
                        git pull

                        echo "🔧 Installing dependencies..."
                        npm install

                        echo "🏗️ Building the app..."
                        npm run build

                        echo "📦 Deploying to Nginx directory..."
                        sudo rm -rf \$NGINX_DIR/build
                        sudo cp -r build \$NGINX_DIR/

                        echo "🔁 Restarting Nginx..."
                        sudo systemctl restart nginx
                    EOF
                """
            }
        }
    }
}
