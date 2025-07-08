pipeline {
    agent any

    environment {
        IMAGE_NAME = "098588167308.dkr.ecr.ap-south-1.amazonaws.com/jenkins:latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/main']],
                          userRemoteConfigs: [[
                              url: 'https://github.com/Ashokkumar-21/python-2.git'
                          ]]
                ])
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'aws-ecr-creds', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                        aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
                        aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
                        aws configure set region ap-south-1

                        aws ecr get-login-password --region ap-south-1 | \
                        docker login --username AWS --password-stdin 098588167308.dkr.ecr.ap-south-1.amazonaws.com
                    '''
                    }
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    sh "docker push ${IMAGE_NAME}"
                }
            }
        }

/*        stage('Pull from DockerHub') {
            steps {
                script {
                    sh "docker pull ${IMAGE_NAME}"
                }
            }
        } */

/*        stage('Run Docker Container') {
            steps {
                script {
                    sh "docker run -itd -p 5000:5000 ${IMAGE_NAME}"
                }
            }
        } */
    }
}
