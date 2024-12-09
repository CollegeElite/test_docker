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
                    if (env.BRANCH_NAME == 'dev') {
                        sh '''
                        # Create a container from the dev image and copy files
                        container_id=$(docker create test_docker:dev)
                        docker cp $container_id:/usr/share/nginx/html ./deploy
                        docker rm $container_id
                        
                        # Configure Git
                        git config --global user.name "CollegeElite"
                        git config --global user.email "oussama.derbel@institutelite-edu.ca"
                        
                        # Checkout the gh-pages branch
                        git checkout gh-pages || git checkout -b gh-pages
                        
                        # Copy the deployment files and push to gh-pages
                        cp -R ./deploy/* .
                        git add .
                        git commit -m "Deploy from dev branch to GitHub Pages"
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