pipeline {
    agent any
    tools{
        maven 'maven'
    }
    parameters {
        choice(name: 'DEPLOY_ENV', choices: ['dev', 'staging', 'production'], description: 'Choose the environment to deploy to')
    }
    environment {
        APP_ENV = "${params.DEPLOY_ENV}"
    }
    stages {
        stage('Build') {
            steps {
                echo "Building the Spring PetClinic application for ${APP_ENV} environment..."
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                echo "Running tests for ${APP_ENV} environment..."
                sh './mvnw test'
            }
        }
        stage('Deploy') {
            when {
                anyOf {
                    expression { params.DEPLOY_ENV == 'dev' }
                    expression { params.DEPLOY_ENV == 'staging' }
                    expression { params.DEPLOY_ENV == 'production' }
                }
            }
            steps {
                echo "Deploying the Spring PetClinic application to ${APP_ENV} environment..."
                // Add deployment steps here, e.g., copying files, running deployment scripts
                // Example:
                // sh './deploy.sh ${APP_ENV}'
            }
        }
    }
    post {
        always {
            echo "Cleaning up..."
            // Add cleanup steps if needed
        }
    }
}
