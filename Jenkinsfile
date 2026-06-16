pipeline {
    agent any

    environment {
        ECR_REGISTRY = "843392124557.dkr.ecr.eu-north-1.amazonaws.com"
        ECR_REPOSITORY = "devops-internship"
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo "Code checked out from branch: ${env.BRANCH_NAME}"
            }
        }

        stage('Build') {
            steps {
                echo "Building application..."
                sh 'pip install pytest'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'pytest tests/ -v'
            }
        }

        stage('Docker Build') {
            steps {
                echo "Building Docker image..."
                sh "docker build -t ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG} ."
            }
        }

        stage('Push') {
            steps {
                echo "Pushing to ECR..."
                sh """
                    aws ecr get-login-password --region eu-north-1 | \
                    docker login --username AWS --password-stdin ${ECR_REGISTRY}
                    docker push ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}
                """
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed."
        }
    }
}
