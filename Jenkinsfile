pipeline {

    agent any

    environment {

        APP_NAME = "springboot-cicd"

        DEV_CONTAINER = "springboot-dev"
        QA_CONTAINER = "springboot-qa"
        PROD_CONTAINER = "springboot-prod"

        DEV_PORT = "8084"
        QA_PORT = "8085"
        PROD_PORT = "8086"

    }

    parameters {

        choice(
            name: 'ENVIRONMENT',
            choices: ['DEV', 'QA', 'PROD'],
            description: 'Choose deployment environment'
        )

    }

    stages {

        stage('Environment Info') {

            steps {

                echo "Selected Environment: ${params.ENVIRONMENT}"

            }
        }

        stage('Build JAR') {

            steps {

                sh 'chmod +x mvnw'

                sh './mvnw clean package'

            }
        }

        stage('Build Docker Image') {

            steps {

                sh 'docker build -t ${APP_NAME} .'

            }
        }

        stage('Deploy Container') {

            steps {

                script {

                    if (params.ENVIRONMENT == 'DEV') {

                        sh 'docker rm -f ${DEV_CONTAINER} || true'

                        sh 'docker run -d --name ${DEV_CONTAINER} -p ${DEV_PORT}:8080 ${APP_NAME}'

                    }

                    else if (params.ENVIRONMENT == 'QA') {

                        sh 'docker rm -f ${QA_CONTAINER} || true'

                        sh 'docker run -d --name ${QA_CONTAINER} -p ${QA_PORT}:8080 ${APP_NAME}'

                    }

                    else {

                        sh 'docker rm -f ${PROD_CONTAINER} || true'

                        sh 'docker run -d --name ${PROD_CONTAINER} -p ${PROD_PORT}:8080 ${APP_NAME}'

                    }

                }

            }
        }

    }
}
