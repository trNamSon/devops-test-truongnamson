pipeline {
    agent any

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
            }
        }

        stage('Build project') {
            steps {
                echo 'Checking website files...'

                sh 'test -f index.html'

                echo 'Build successful!'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploy stage started...'
                echo 'Website will be deployed to Vercel.'
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