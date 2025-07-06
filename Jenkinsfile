pipeline {
    agent any

    environment {
        EC2_USER = 'ubuntu'
        EC2_HOST = '65.0.73.128'  
        SSH_CREDENTIAL_ID = 'ec2-ssh-key' git
    }

    stages {
        stage("Build") {
            steps {
                echo "Start Building"
                sh "npm install"
                sh "npm run build"
                echo "Building Completed"
            }
        }

        stage("Deploy") {
            steps {
                echo "Starting Deployment on EC2..."

                // Use sshagent to provide the private key
                sshagent([env.SSH_CREDENTIAL_ID]) {
                    sh '''
                        echo "Copying build artifacts to EC2..."
                        scp -o StrictHostKeyChecking=no -r dist/* $EC2_USER@$EC2_HOST:/var/www/html

                        echo "Restarting web server on EC2..."
                        ssh -o StrictHostKeyChecking=no $EC2_USER@$EC2_HOST 'sudo systemctl restart nginx'
                    '''
                }

                echo "Deployment Completed"
            }
        }
    }
}
