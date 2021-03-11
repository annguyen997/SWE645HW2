pipeline {
    agent any
    environment {
        DOCKERHUB_PASS = credentials('docker-pass')
    }
    stages {
        stage('Building the Student Survey Image') {
            steps {
                script {
                    checkout scm
                    sh 'rm -rf *.war'
                    sh 'jar -cvf HW_2.war -C Assignment2/hw_2/WebContent/ .'
                    sh "docker pull nginx"
                    def customImage = docker.build("risshie/swe642:${BUILD_TIMESTAMP}")
                }
            }
        }
        stage('Pushin Image to DockerHub') {
            steps {
                script {
                    sh 'docker push risshie/swe642:${BUILD_TIMESTAMP}'
                }
            }
        }
        stage('Deploying to Rancher as single pod') {
            steps {
                sh 'kubectl set image deployment/pipeline-test pipeline-test=risshie/swe642:${BUILD_TIMESTAMP} -n jenkins-pipeline'
            }
        }
        stage("Deploying to Rancher as with load balancer") {
            steps {
                sh 'kubectl set image deployment/survey-server survey-server=risshie/swe642:${BUILD_TIMESTAMP} -n jenkins-pipeline'
            }
        }

    }
}
