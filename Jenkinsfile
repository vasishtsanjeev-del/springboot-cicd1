pipeline {

    agent any

    environment {

        APP_NAME = "springboot-cicd"

        IMAGE_TAG = "${BUILD_NUMBER}"

        DEV_CONTAINER = "springboot-dev"
        PROD_CONTAINER = "springboot-prod"

        DEV_PORT = "8084"
        PROD_PORT = "8086"

        GITHUB_PAT = credentials('github-pat')

    }

    stages {

        stage('Environment Information') {

            steps {

                echo 'Starting Production Style CI/CD Pipeline'

            }
        }

        stage('Checkout Source Code') {

            steps {

                echo 'Checking out source code from GitHub'

                checkout scm

            }
        }

        stage('Credentials Test') {

            steps {

                echo 'GitHub credentials loaded successfully'

            }
        }

        stage('Build Information') {

            steps {

                echo "Application Name: ${APP_NAME}"

                echo "Docker Image Tag: ${IMAGE_TAG}"

                echo "Jenkins Build Number: ${BUILD_NUMBER}"

            }
        }

        stage('Build JAR') {

            steps {

                echo 'Building Spring Boot Application'

                sh 'chmod +x mvnw'

                sh './mvnw clean package'

            }
        }

        stage('Build Docker Image') {

            steps {

                echo 'Building Versioned Docker Image'

                sh 'docker build -t ${APP_NAME}:${IMAGE_TAG} .'

            }
        }

        stage('Deploy DEV') {

            steps {

                echo 'Automatically Deploying to DEV Environment'

                sh 'docker rm -f ${DEV_CONTAINER} || true'

                sh 'docker run -d --name ${DEV_CONTAINER} -p ${DEV_PORT}:8080 ${APP_NAME}:${IMAGE_TAG}'

            }
        }

        stage('Production Approval') {

            steps {

                input {

                    message "Approve Deployment to Production?"

                    ok "Deploy PROD"

                }

            }
        }

        stage('Deploy PROD') {

            steps {

                echo 'Deploying to Production Environment'

                sh 'docker rm -f ${PROD_CONTAINER} || true'

                sh 'docker run -d --name ${PROD_CONTAINER} -p ${PROD_PORT}:8080 ${APP_NAME}:${IMAGE_TAG}'

            }
        }

    }

    post {

        success {

            echo 'Pipeline executed successfully'

        }

        failure {

            echo 'Pipeline execution failed'

        }

        always {

            echo 'Pipeline execution completed'

        }

    }

}
