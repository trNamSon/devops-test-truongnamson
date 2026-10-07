pipeline {
    agent any

    environment {
        TELEGRAM_BOT_TOKEN = credentials('telegram-bot-token')
        TELEGRAM_CHAT_ID   = credentials('telegram-chat-id')
        VERCEL_TOKEN       = credentials('vercel-token')
        PROJECT_NAME       = 'devops-test'
        BRANCH_NAME        = 'main'
    }

    stages {
        stage('Notify Start') {
            steps {
                sh '''
                curl -s -X POST "https://api.telegram.org/bot$TELEGRAM_BOT_TOKEN/sendMessage" \
                  -d chat_id=$TELEGRAM_CHAT_ID \
                  -d text="🚀 DEPLOY STARTED%0AProject: $PROJECT_NAME%0ABranch: $BRANCH_NAME"
                '''
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build project') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                npm install --global vercel@latest
                vercel pull --yes --environment=production --token=$VERCEL_TOKEN
                vercel build --prod --token=$VERCEL_TOKEN
                vercel deploy --prebuilt --prod --token=$VERCEL_TOKEN > deploy_url.txt
                '''
                script {
                    env.DEPLOY_URL = sh(script: "tail -n 1 deploy_url.txt", returnStdout: true).trim()
                }
            }
        }
    }

    post {
        success {
            sh '''
            curl -s -X POST "https://api.telegram.org/bot$TELEGRAM_BOT_TOKEN/sendMessage" \
              -d chat_id=$TELEGRAM_CHAT_ID \
              -d text="✅ DEPLOY SUCCESS%0AProject: $PROJECT_NAME%0ABranch: $BRANCH_NAME%0AURL: $DEPLOY_URL"
            '''
        }
        failure {
            sh '''
            curl -s -X POST "https://api.telegram.org/bot$TELEGRAM_BOT_TOKEN/sendMessage" \
              -d chat_id=$TELEGRAM_CHAT_ID \
              -d text="❌ DEPLOY FAILED%0AProject: $PROJECT_NAME%0ABranch: $BRANCH_NAME%0APlease check Jenkins."
            '''
        }
    }
}
