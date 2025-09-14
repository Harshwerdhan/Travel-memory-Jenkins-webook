// The Final Travel Memory Jenkinsfile
pipeline {
    // 1. Use the agent we configured
    agent { label 'master' }

    // 2. Define the non-secret variables
    environment {
        app_name = 'travel_memory' // CHANGE THIS
    }
    
    // 3. Define the parameters for flexibility (for manual runs)
    parameters {
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Check this to run unit tests.')
    }

    // 4. Define the automatic trigger for this pipeline
    triggers {
        // This enables the pipeline to be started automatically by a GitHub webhook on a 'git push'
        githubPush()
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Harshwerdhan/Travel-memory-Jenkins-webook'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Run npm install inside the 'backend' folder
                dir('backend') {
                    sh 'npm install'
                }
            }
        }

        stage('Run Tests') {
            // A conditional stage based on our parameter
            when { expression { return params.RUN_TESTS } }
            steps {
                dir('backend') {
                    sh 'npm test'
                }
            }
        }
            // AWS things
        stage('Login to AWS and Check S3 buckets') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', 
                  credentialsId: 'AWS_SECRET_TM']]) {
                    echo "checking AWS S3 bucket.."
                    sh "aws s3 ls"
                    echo "${env.app_name} running on AWS"
                }
            }
        }
    }
    
    // Post-build actions for cleanup
    post {
        always {
            echo "Pipeline finished. Cleaning up."
            // Always logout to be safe
            echo 'Installed and tested the application'
        }
        success {
            echo "Checked S3 Buckets"
        }
    }
}
