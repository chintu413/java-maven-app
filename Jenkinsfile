#!/usr/bin.env groovy

library identigier:'jenkins-shared-library@master', retriver: modernSCM(
        [$class : 'GitSCMSource',
        remote: "https://github.com/chintu413/jenkins-shared-library.git",
        credentialsId: 'gitlab-credentials']
)

pipeline {   
    agent any
    tools {
        maven 'Maven'
    }
    environment {
        IMAGE_NAME = 'chintu413/my-first-docker-image'
    }
    stages {
        stage("build app") {
            steps {
                script {
                    stage('build app') {
                        steps {
                            echo 'building application jar...'
                            buildJar()
                        }


                    }
            }
        }
        stage("build") {
            steps {
                script {
                    echo 'building the docker image...'
                    buildImage(env.IMAGE_NAME)
                    dockerLogin()
                    dockerPush(env.IMAGE_NAME)

                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    echo 'deploying docker image to EC2...'
                    def dockerCmd = 'docker run -p 3080:3080 -d nanatwn/demo-app:1.0'
                    sshagent(['ec2-server-key']) {
                       sh "ssh -o StrictHostKeyChecking=no ec2-user@18.184.54.160 ${dockerCmd}"      
                    }
                }
            }
        }               
    }
} 
