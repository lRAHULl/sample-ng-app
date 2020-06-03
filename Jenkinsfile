// Define the variables
def dockerPublisherName = "rahulraju"
def dockerRepoName = "sample-ng-app"

def gitRepoName = "https://github.com/lRAHULl/sample-ng-app.git"
def gitBranch = "master"

def customLocalImage = "sample-ng-app-image"

properties([pipelineTriggers([githubPush()])])
pipeline {
    agent {
        node {
            label 'linux-slave'
        }
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${gitBranch}", url: "${gitRepoName}"
            }
        }

        // stage('Code Analysis And Testing') {
        //     parallel {
        //         stage('Static Code Analysis') {
        //             steps {
        //                 sh 'chmod +x gradlew'
        //                 // sh './gradlew checkstyle'
        //             }
        //         }

        //         stage('Unit Testing') {
        //             steps {
        //                 echo "Testing"
        //                 // sh './gradlew test'
        //             }
        //         }
        //     }
        // }

        stage('Build') {
            steps {
                // sh './gradlew bootjar'
                sh "docker build -t ${customLocalImage} ."
                sendSlackMessage "Build Successul"
            }
        }

        stage('Publish') {
            steps {
                sh "docker tag ${customLocalImage} ${dockerPublisherName}/${dockerRepoName}:alpha-0.0.${BUILD_NUMBER}"
                sh "docker push ${dockerPublisherName}/${dockerRepoName}"
                sendSlackMessage "Publish Successul"
            }
        }

        stage('Deploy') {
            steps {
                sh """
                    docker network rm simple-ng-overlay || true
                    docker network create --driver overlay --attachable simple-ng-overlay
                    docker service rm simple-ng-service || true
                    docker service create --replicas 3 --network simple-ng-overlay --name simple-ng-service -p 9090:80 ${dockerPublisherName}/${dockerRepoName}
                """
            }
        }

        stage('Clean') {
            steps {
                sh "docker rmi ${customLocalImage} || true"
            }
        }

    }
}
void sendSlackMessage(String message) {
    slackSend botUser: true, channel: 'private_s3_file_upload', failOnError: true, message: "Message From http://3.231.228.54:8080/: \n${message}", teamDomain: 'codaacademy2020', tokenCredentialId: 'coda-academy-slack'
}
