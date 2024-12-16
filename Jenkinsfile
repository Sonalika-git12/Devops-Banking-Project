pipeline {
    agent any
    
    tools {
        maven 'maven'
    }
    
    stages {
        stage('Clone Repo') {
            steps {
               git 'https://github.com/Git-Geetansh/star-agile-banking-finance.git'
            }
        }
        
        stage('Test Code') {
            steps {
                sh 'mvn test'
            }
        }
        
        stage('Build Code') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t financeme$BUILD_NUMBER .'
            }
        }
        
        stage('Deploy Container') {
            steps {
                script {
                    // Stop and remove the old container if running
                    sh '''
                    echo "Checking for existing container..."
                    if [ $(docker ps -q --filter "name=financeme_container") ]; then
                        echo "Stopping existing container..."
                        docker stop financeme_container
                        echo "Removing existing container..."
                        docker rm financeme_container
                    else
                        echo "No existing container found."
                    fi
                    '''
                    
                    // Run the new container
                    sh '''
                    echo "Running new container..."
                    docker run -d --name financeme_container -p 8081:8081 financeme$BUILD_NUMBER
                    '''
                }
            }
        }
    }
}
