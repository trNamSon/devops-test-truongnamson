pipeline {
    agent any

    tools {
        nodejs 'NodeJS-22'
    }

    environment {
        PROJECT_NAME = 'devops-test'
        BRANCH_NAME = 'main'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code from GitHub...'

                git branch: 'main',
                    url: 'https://github.com/trNamSon/devops-test-truongnamson.git'
            }
        }

        stage('Install dependencies') {
            steps {
                echo 'Static HTML project - no dependencies required.'

                sh 'node --version'
                sh 'npm --version'
            }
        }

        stage('Build project') {
            steps {
                echo 'Checking website files...'

                sh 'test -f index.html'

                echo 'Build successful!'
            }
stage('Deploy') {
    steps {
        withCredentials([
            string(
                credentialsId: 'vercel-token',
                variable: 'VERCEL_TOKEN'
            ),
            string(
                credentialsId: 'telegram-token',
                variable: 'TELEGRAM_TOKEN'
            ),
            string(
                credentialsId: 'telegram-chat-id',
                variable: 'TELEGRAM_CHAT_ID'
            )
        ]) {

            sh '''
                echo "Sending DEPLOY STARTED notification..."

                curl -sS -X POST \
                    "https://api.telegram.org/bot${TELEGRAM_TOKEN}/sendMessage" \
                    --data-urlencode "chat_id=${TELEGRAM_CHAT_ID}" \
                    --data-urlencode "text=🚀 DEPLOY STARTED
Project: devops-test
Branch: main"

                echo "Installing Vercel CLI..."

                npm install -g vercel

                echo "Vercel CLI version:"
                vercel --version

                echo "Deploying to Vercel..."

                vercel deploy --prod \
                    --token "$VERCEL_TOKEN" \
                    --yes
            '''
        }
    }
}
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Please check Jenkins Console Output.'
        }
    }
}