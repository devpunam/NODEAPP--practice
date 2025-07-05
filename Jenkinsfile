pipeline {
    agent any

    stages {
        stage("Build") {
            steps {git
                echo "Start Building"
                sh "npm install"
                sh "npm run build"
                echo "Building Completed"
            }
        }
    }
}
