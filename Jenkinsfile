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
                sshagent([ec2-ssh-key]) {
                    sh '''
                        echo "Copying build artifacts to EC2
                    scp -o StrictHostChecking=no -r dist/ ubuntu@65.0.73.128:/var/www/html
                    echo "Restarting web server on EC2"
                    ssh -o StrictHostKeyChecking=no ubuntu@65.0.73.128 'sudo systemctl restart ngnix'

                    '''
                }

                echo "Deployment Completed"
            }
        }
    }
}

