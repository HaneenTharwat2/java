@Library('my-shared-lib') _

pipeline {
    agent any

    tools {
        jdk "java-21"
    }

    environment {
        DOCKER_CREDENTIALS = credentials('dockerhub-user') // Use Jenkins credentials ID
    }

    parameters {
        string defaultValue: '${BUILD_NUMBER}', description: 'Enter the version of the docker image', name: 'VERSION'
        choice choices: ['true', 'false'], description: 'Skip test', name: 'TEST'
    }

    stages {
        stage("VM info") {
            steps {
                script {
                    def VM_IP = vmIp()
                    sh "echo ${VM_IP}"
                }
            }
        }

        stage("Build java app") {
            steps {
                script {
                    sayHello "ITI"
                }
                sh "mvn clean package install -Dmaven.test.skip=${TEST}"
            }
        }

        stage("Build docker image") {
            steps {
                script {
                    def dockerx = new org.iti.docker()
                    dockerx.build("java", "${VERSION}")
                }
                sh "docker login -u ${DOCKER_CREDENTIALS_USR} -p ${DOCKER_CREDENTIALS_PSW}"
            }
        }

        stage("Push docker image") {
            steps {
                script {
                    def dockerx = new org.iti.docker()
                    dockerx.login("${DOCKER_CREDENTIALS_USR}", "${DOCKER_CREDENTIALS_PSW}")
                    dockerx.push("${DOCKER_CREDENTIALS_USR}", "${VERSION}")
                }
            }
        }
    }

    post {
        always {
            sh 'echo "Running cleanup or post actions..."'
        }
        failure {
            sh 'echo "The build failed. Taking some actions..."'
        }
    }
}

    }
}

            node {
                sh 'echo "The build failed. Taking some actions..."'
            }
        }
    }
}

