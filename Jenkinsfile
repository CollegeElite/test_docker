pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo "Checked out branch: ${env.BRANCH_NAME}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'dev') {
                        sh 'docker build -t test_docker:dev -f dockerfile.dev .'
                    } else if (env.BRANCH_NAME == 'staging') {
                        sh 'docker build -t test_docker:staging -f dockerfile.staging .'
                    } else if (env.BRANCH_NAME == 'prod') {
                        sh 'docker build -t test_docker:prod -f dockerfile.prod .'
                    }
                }
            }
        }

        stage('Test Docker Image') {
            steps {
                script {
                    sh 'echo "Running tests for Docker image..."'
                }
            }
        }

        stage('Deploy to GitHub Pages') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'prod') {
                        sh '''
                        docker run --rm test_docker:prod
                        git checkout gh-pages
                        cp -R /usr/share/nginx/html/* .
                        git add .
                        git commit -m "Deploy to GitHub Pages"
                        git push origin gh-pages
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished for branch: ${env.BRANCH_NAME}"
        }
    }
}
