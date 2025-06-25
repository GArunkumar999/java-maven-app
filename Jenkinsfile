pipeline{
    agent any
    tools{
        maven 'Maven3'
    }
    environment {
        IMAGE_NAME = 'arun596/java-maven:latest'
    }

    stages{
        stage('clean workspace'){
            steps{
                cleanWs()

            }
        }
        stage('clone git repo'){
            steps{
                git branch: 'main', url: 'https://github.com/GArunkumar999/java-maven-app.git'
            }
        }
        stage('build code'){
            steps{
                script{
                    sh 'mvn clean package'
                }
            }
        }
        stage('build image and push to docker hub'){
            steps{
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh """
                            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                            docker build -t $IMAGE_NAME .
                            docker push $IMAGE_NAME
                        """
                    }
                }
              
            }
        }
        stage('create container'){
            steps{
                script{
                    sh 'docker run -d --name java-maven -p 9000:8080 $IMAGE_NAME'
                }
            }
        }
    }
}
