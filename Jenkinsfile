pipeline {
    agent {label 'jenkins-user-agent-1'}
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Harshwerdhan/Travel-memory-Jenkins-webook.git'
            }
        }
        stage('Install') {
            steps {
                sh 'cd backend; npm install'
            }
        }
        stage('Build') {
            steps {
                sh 'cd backend; npm run build'
            }
        }
    }

}






