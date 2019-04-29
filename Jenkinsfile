#!/usr/bin/env groovy

pipeline {
    agent { node { label 'Host_Node' } }
    
    environment {
            def image = null;
    }
        stages {
            stage('Building App') {
                steps {
                    sh 'chmod +x Slack_bot.sh';
                    sh './Slack_bot.sh Building App';
                    script {  image = docker.build("koomely/couchpotato:${env.BUILD_NUMBER}") }
                }
            }
            stage('Send to Docker-Hub') {
                steps {
                   sh './Slack_bot.sh Sending to Docker-Hub';
                    docker.withRegistry('https://docker.io', 'docker-id') {
                    script {
                       image.push(); 
                           }
                    }
                }
            }
                stage('Running Test'){
                    steps {
                     sh './Slack_bot.sh Uploaded succesfully to Docker-Hub. Starting deployment!';
                    }
                }
                
            }
        }
    
