pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/rahul-chaudhari-collab/java-cicd-docker-deployment.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                        mvn sonar:sonar \
                          -Dsonar.projectKey=java-cicd-docker-deployment \
                          -Dsonar.projectName=java-cicd-docker-deployment
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t java-cicd-app:v1 .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f java-cicd-container || true
                    docker run -d --name java-cicd-container \
                        -p 8081:8080 java-cicd-app:v1
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'docker ps'
            }
        }
    }
}
