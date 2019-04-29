#!/usr/bin/env groovy

pipeline {
    agent { node { label 'Host_Node' } }
    
    environment {
            def image = null;
            registryCredential = "docker-id"
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
                    script {
                       image.push(); 
                           }
                }
            }
        }
    }
