pipeline {

    agent any

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

                echo 'Building Spring Boot Application'

                sh 'chmod +x mvnw'

                sh './mvnw clean package'

            }
        }

        stage('Build Docker Image') {

            steps {

                echo 'Building Docker Image'

                sh 'docker build -t springboot-cicd .'

            }
        }

        stage('Deploy Container') {

            steps {

                script {

                    if (params.ENVIRONMENT == 'DEV') {

                        sh 'docker rm -f springboot-dev || true'

                        sh 'docker run -d --name springboot-dev -p 8084:8080 springboot-cicd'

                    }

                    else if (params.ENVIRONMENT == 'QA') {

                        sh 'docker rm -f springboot-qa || true'

                        sh 'docker run -d --name springboot-qa -p 8085:8080 springboot-cicd'

                    }

                    else {

                        sh 'docker rm -f springboot-prod || true'

                        sh 'docker run -d --name springboot-prod -p 8086:8080 springboot-cicd'

                    }

                }

            }
        }

    }
}
