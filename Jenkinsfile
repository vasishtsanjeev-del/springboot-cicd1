pipeline {

    agent any

    environment {

        APP_NAME = "springboot-cicd"

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

                echo 'Checking out source code'

                checkout scm

            }
        }

        stage('Credentials Test') {

            steps {

                echo 'GitHub credentials loaded successfully'

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

                echo 'Building Docker Image'

                sh 'docker build -t ${APP_NAME} .'

            }
        }

        stage('Deploy DEV') {

            steps {

                echo 'Automatically Deploying to DEV'

                sh 'docker rm -f ${DEV_CONTAINER} || true'

                sh 'docker run -d --name ${DEV_CONTAINER} -p ${DEV_PORT}:8080 ${APP_NAME}'

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

                sh 'docker run -d --name ${PROD_CONTAINER} -p ${PROD_PORT}:8080 ${APP_NAME}'

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
