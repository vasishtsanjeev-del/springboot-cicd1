pipeline {

    agent { label 'docker-agent' }

    environment {

        APP_NAME = "springboot-cicd"

        IMAGE_TAG = "${BUILD_NUMBER}"

        DEV_CONTAINER = "springboot-dev"

        DEV_PORT = "8084"

        GITHUB_PAT = credentials('github-pat')
       
        DOCKERHUB_USERNAME = "san91"

    }

    stages {

        stage('Environment Information') {

            steps {

                echo 'Starting CI/CD Pipeline'

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
        stage('Parameter Information') {

    steps {

        echo "Environment: ${params.ENVIRONMENT}"

        echo "Version: ${params.VERSION}"

        echo "Deploy Enabled: ${params.DEPLOY}"

    }
}

        stage('Build JAR') {

            steps {

                echo 'Building Spring Boot Application'

                sh 'chmod +x mvnw'

                sh './mvnw clean package'

            }
        }
         
         stage('Parallel Checks') {

    parallel {

        stage('Code Check') {

            steps {

                echo 'Running Code Validation'

            }
        }

        stage('Security Check') {

            steps {

                echo 'Running Security Validation'

            }
        }

        stage('Quality Check') {

            steps {

                echo 'Running Quality Validation'

            }
        }

    }
}
        
         stage('Archive Artifact') {

               steps {

                   echo 'Archiving Spring Boot JAR'

                   archiveArtifacts artifacts: 'target/*.jar', fingerprint: true

    }
}

        stage('Build Docker Image') {

            steps {

                echo 'Building Versioned Docker Image'

                sh 'docker build -t ${APP_NAME}:${IMAGE_TAG} .'

            }
        }

           stage('Push Docker Image') {

    steps {

        echo 'Logging into DockerHub'

        withCredentials([usernamePassword(

            credentialsId: 'dockerhub-creds',
            usernameVariable: 'DOCKER_USER',
            passwordVariable: 'DOCKER_PASS'

        )]) {

            sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'

            sh 'docker tag ${APP_NAME}:${IMAGE_TAG} ${DOCKERHUB_USERNAME}/${APP_NAME}:${IMAGE_TAG}'

            sh 'docker push ${DOCKERHUB_USERNAME}/${APP_NAME}:${IMAGE_TAG}'

        }

    }
}

        stage('Deploy DEV') {

            steps {

                echo 'Automatically Deploying to DEV Environment'

                sh 'docker rm -f ${DEV_CONTAINER} || true'

                sh 'docker run -d --name ${DEV_CONTAINER} -p ${DEV_PORT}:8080 ${APP_NAME}:${IMAGE_TAG}'

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
