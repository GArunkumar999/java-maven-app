pipeline{
    agent any
    tools{
        maven 'Maven3'
    }
    environment {
        IMAGE_NAME = 'arun596/java-maven:latest'
        CONTAINER_NAME = 'java-maven'
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
        // stage('remove container'){
        //     steps{
        //         script{
        //             sh"""
        //             docker stop $CONTAINER_NAME
        //             docker rm $CONTAINER_NAME
        //             """
        //         }
        //     }

        // }
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
                    sh 'docker run -d --name $CONTAINER_NAME-$BUILD_NUMBER -p 9000:8080 $IMAGE_NAME'
                }
            }
        }
    }
    post {
        always {
            script {
              def buildStatus = currentBuild.currentResult
              def buildUser = currentBuild.getBuildCauses('hudson.model.Cause$UserIdCause')[0]?.userId ?: 'Github User'
            
                emailext (
                 subject: "Pipeline ${buildStatus}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: """
                    <p>This is a Jenkins maven CICD pipeline status.</p>
                    <p>Project: ${env.JOB_NAME}</p>
                    <p>Build Number: ${env.BUILD_NUMBER}</p>
                    <p>Build Status: ${buildStatus}</p>
                    <p>Started by: ${buildUser}</p>
                    <p>Build URL: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                  """,
                  to: 'giddaluruarunkumar9@gmail.com',
                  from: 'giddaluruarunkumar9@gmail.com',
                  replyTo: 'giddaluruarunkumar9@gmail.com',
                  mimeType: 'text/html',
                  attachmentsPattern: 'trivyfs.txt,trivyimage.txt'
                )
            }
        }

    }
}
