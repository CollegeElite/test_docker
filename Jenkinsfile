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

        stage('Push to Repository') {
            steps {
                script {
                    sh '''
                    # Run the Docker container and extract files
                    container_id=$(docker create test_jenkins:${env.BRANCH_NAME})
                    docker cp $container_id:/usr/share/nginx/html ./deploy
                    docker rm $container_id

                    # Configure Git for committing changes
                    # Configure Git
                    git config --global user.name "CollegeElite"
                    git config --global user.email "oussama.derbel@intitutelite-edu.ca"
                    

                    # Add changes to the repository
                    mkdir -p ./output/${env.BRANCH_NAME}
                    cp -R ./deploy/* ./output/${env.BRANCH_NAME}
                    git add ./output/${env.BRANCH_NAME}
                    git commit -m "Add output files for branch ${env.BRANCH_NAME}"
                    git push origin ${env.BRANCH_NAME}
                    '''
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
