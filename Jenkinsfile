pipeline{
    agent any
    tools{
        maven 'Maven3'
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
    }
}
