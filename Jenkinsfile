pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Java Version') {
            steps {
                sh 'java -version'
            }
        }

        stage('Maven Version') {
            steps {
                sh 'mvn -version'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t employee-jenkins-demo:1.0 .'
            }
        }

        stage('Docker Run') {
            steps {
                sh '''
                    docker rm -f employee-app || true
                    docker run -d \
                      --name employee-app \
                      -p 8081:8080 \
                      employee-jenkins-demo:1.0
                '''
            }
        }

        stage('Verify Application') {
            steps {
                sh 'sleep 10'
                sh 'curl -f http://localhost:8081/employees'
            }
        }
    }

    post {
        success {
            echo 'CI/CD pipeline completed successfully!'
        }

        failure {
            echo 'CI/CD pipeline failed!'
        }
    }
}
