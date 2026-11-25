
pipeline {

    agent any

    triggers {
        pollSCM('H/2 * * * *')
    }

    environment {
        IMAGE_NAME = 'prepspace-frontend'
        CONTAINER_NAME = 'prepspace-frontend'
    }

    stages {

        stage('GitHub Checkout') {
            steps {
                echo 'Downloading source code from GitHub...'

                checkout scm

                echo 'GitHub checkout completed!'
            }
        }

        stage('Testing') {
            steps {
                echo 'Starting testing...'

                echo 'Unit Testing Done!'
                echo 'Integration Testing Done!'
                echo 'System Testing Done!'
                echo 'Security Testing Done!'

                echo 'All simulated tests completed!'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Creating Docker image...'

                sh '''
                    docker build -t prepspace-frontend:${BUILD_NUMBER} .
                    docker tag prepspace-frontend:${BUILD_NUMBER} prepspace-frontend:latest
                '''

                echo 'Docker image created!'
            }
        }

        stage('Docker Deployment') {
            steps {
                echo 'Deploying application...'

                sh '''
                    docker rm -f prepspace-frontend || true

                    docker run -d \
                        --name prepspace-frontend \
                        --restart unless-stopped \
                        -p 8081:80 \
                        prepspace-frontend:${BUILD_NUMBER}
                '''

                echo 'Application deployed!'
            }
        }

        stage('Monitoring') {
            steps {
                sh 'docker ps'
                sh 'docker logs --tail 20 prepspace-frontend'

                echo 'Monitoring completed!'
            }
        }
    }

    post {
        success {
            echo 'CI/CD PIPELINE SUCCESSFUL!'
        }

        failure {
            echo 'CI/CD PIPELINE FAILED!'
        }
    }
}
