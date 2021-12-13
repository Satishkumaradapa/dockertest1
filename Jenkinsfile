pipeline {
    environment {
        registry = '8074764785/dockertest1'
        registryCredential = '8074764785'
        dockerhost = '10.1.1.165'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning our Git') {
            steps {
                git 'https://github.com/Satishkumaradapa/dockertest1.git'
            }
        }
        stage('Building our image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":v$BUILD_NUMBER"
                }
            }
        }
        stage('Push Image To DockerHUB') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Cleaning up') {
            steps {
                sh "docker rmi $registry:v$BUILD_NUMBER"
            }
        }
        stage('dockerhost') {
            steps {
                sh "docker service create --name testing1 -p 8100:80 $registry:v$BUILD_NUMBER"
            }
        }
        stage('Verifying The Deployment') {
            steps {
                sh 'curl http://$dockerhost:8100 || exit 1'
                }
        }
    }
}
