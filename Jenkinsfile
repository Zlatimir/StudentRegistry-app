pipeline {
    agent any

    environment {
        NODE_VERSION = '18'
        NVM_DIR = "${HOME}/.nvm"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Node.js via NVM') {
            steps {
                sh '''
                    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
                    export NVM_DIR="$HOME/.nvm"
                    [ -s "$NVM_DIR/nvm.sh" ] && \\. "$NVM_DIR/nvm.sh"
                    nvm install $NODE_VERSION
                    nvm use $NODE_VERSION
                    node -v
                    npm -v
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    export NVM_DIR="$HOME/.nvm"
                    [ -s "$NVM_DIR/nvm.sh" ] && \\. "$NVM_DIR/nvm.sh"
                    nvm use $NODE_VERSION
                    npm install
                '''
            }
        }

        stage('Start Application') {
            steps {
                sh '''
                    export NVM_DIR="$HOME/.nvm"
                    [ -s "$NVM_DIR/nvm.sh" ] && \\. "$NVM_DIR/nvm.sh"
                    nvm use $NODE_VERSION
                    nohup npm start &
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    export NVM_DIR="$HOME/.nvm"
                    [ -s "$NVM_DIR/nvm.sh" ] && \\. "$NVM_DIR/nvm.sh"
                    nvm use $NODE_VERSION
                    npm test
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
