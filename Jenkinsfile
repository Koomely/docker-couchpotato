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
                    script {  image = docker.build("koomely/couchpotato:latest") }
                }
            }
            stage('Send to Docker-Hub') {
                steps {
                   sh './Slack_bot.sh Sending to Docker-Hub';
                    
                    script {docker.withRegistry('', 'docker-id') {
                       image.push(); 
                           }
                    }
                }
            }
            
            stage('Running Docker Image Tests') {
                steps {
                        sh './Slack_bot.sh Testing Docker image';
                        sh 'dgoss run -p 666:5050 koomely/couchpotato:latest'                    
                }
            }
                stage('Pushing to Deployment!'){
                    steps {
                     sh './Slack_bot.sh Uploaded succesfully to Docker-Hub. Starting deployment!';
                     sh 'cd /home/ubuntu/src/yamls; ansible-playbook -i ansible_hosts run_pod.yml';
                    }
                }
            stage('Finished Deployment'){
                steps{
                 sh './Slack_bot.sh Finished Deployment!!!';   
                }
            }
                
            }
        }
    
