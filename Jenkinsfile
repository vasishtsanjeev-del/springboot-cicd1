pipeline {

    agent any

    stages {

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

                echo 'Deploying Container'

                sh 'docker rm -f springboot-container || true'

                sh 'docker run -d --name springboot-container -p 8084:8080 springboot-cicd'

            }
        }

    }
}
