pipeline {
    agent any

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/yashiz4u-lgtm/blue-green-project.git'
            }
        }

        stage('Build Blue Image') {
            steps {
                sh '''
                cp app-blue.py app.py
                docker build -t bluegreen-app:blue .
                '''
            }
        }

        stage('Build Green Image') {
            steps {
                sh '''
                cp app-green.py app.py
                docker build -t bluegreen-app:green .
                '''
            }
        }

        stage('Deploy Blue') {
            steps {
                sh 'kubectl apply -f blue-deployment.yaml'
            }
        }

        stage('Deploy Green') {
            steps {
                sh 'kubectl apply -f green-deployment.yaml'
            }
        }

        stage('Deploy Service') {
            steps {
                sh 'kubectl apply -f service.yaml'
            }
        }

        stage('Switch Traffic') {
            steps {
                sh '''
                kubectl patch service my-service -p '{"spec":{"selector":{"app":"myapp","version":"green"}}}'
                '''
            }
        }
    }
}