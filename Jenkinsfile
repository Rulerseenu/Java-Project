pipeline {
    agent any
    
    tools {
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
                sh 'mvn test'
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
                // Make sure to use the correct deploy step if you're using a plugin
                deploy adapters: [tomcat7(credentialsId: 'tomcat-username-password', path: '', url: 'http://44.204.26.124:8080')],
                        contextPath: '', war: '**/*.war'
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
