pipeline {
    agent any

    environment {
        MONGODB_URI = credentials('mongodb-uri')
        TOKEN_KEY = credentials('token-key')
        EMAIL = credentials('email')
        PASSWORD = credentials('password')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Client Tests') {
            steps {
                dir('client') {
                    sh 'npm install'
                    sh 'npm test'
                }
            }
        }
        
        stage('Server Tests') {
            steps {
                dir('server') {
                    sh 'npm install'
                    sh 'export MONGODB_URI=$MONGODB_URI'
                    sh 'export TOKEN_KEY=$TOKEN_KEY'
                    sh 'export EMAIL=$EMAIL'
                    sh 'export PASSWORD=$PASSWORD'
                    sh 'npm test'
                }
            }
        }
        
        stage('Build Images') {
            steps {
                sh 'docker build -t rakeshpotnuru/productivity-app:client client'
                sh 'docker build -t rakeshpotnuru/productivity-app:server server'
                sh 'docker build -t rakeshpotnuru/productivity-app:nginx nginx'
            }
        }
        
        // stage('Push Images') {
        //     steps {
        //         withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
        //             sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
        //             sh 'docker push rakeshpotnuru/productivity-app:client'
        //             sh 'docker push rakeshpotnuru/productivity-app:server'
        //             sh 'docker push rakeshpotnuru/productivity-app:nginx'
        //         }
        //     }
        // }
        
        // stage('Deploy to Elastic Beanstalk') {
        //     steps {
        //         withCredentials([file(credentialsId: 'aws-credentials', variable: 'AWS_CREDENTIALS_FILE')]) {
        //             sh 'curl https://s3.amazonaws.com/aws-cli/awscli-bundle.zip -o awscli-bundle.zip'
        //             sh 'unzip awscli-bundle.zip'
        //             sh './awscli-bundle/install -b ~/bin/aws'
        //             sh 'chmod +x ~/bin/aws'
        //             sh 'export PATH=$PATH:~/bin'
        //             sh 'eval "$(aws ecr get-login --no-include-email --region us-east-1)"'
        //             sh 'docker-compose up -d'
        //         }
        //     }
        // }
    }
}