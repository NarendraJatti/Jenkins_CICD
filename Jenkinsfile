pipeline {
    agent any 

    environment {
        IMAGE_NAME = 'narendra22/test-app'
        IMAGE_TAG = "${IMAGE_NAME}:${env.GIT_COMMIT}"
    }

    stages {

        stage('setup') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('tests') {
            steps {
                sh "pytest"
            }
        }

        stage('login to docker hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo ${PASSWORD} | docker login -u ${USERNAME} --password-stdin'
                    echo 'Docker login succeeded'
                }
            }
        }

        stage('build docker image') {
            steps {
                sh 'docker build -t ${IMAGE_TAG} .'
                echo 'Docker image build successful'
                sh 'docker image ls'
            }
        }

        stage('push docker image') {
            steps {
                sh 'docker push ${IMAGE_TAG}'
                echo 'Docker image pushed successfully'
            }
        }
    }
}
