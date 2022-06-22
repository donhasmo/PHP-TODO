pipeline {
    agent any

    stages {

        stage('Initial Cleanup') {
            steps {
            dir("${WORKSPACE}") {
              deleteDir()
            }
          }
        }

        stage ('Checkout Repo'){
            steps {
            git branch: 'main', url: 'https://github.com/donhasmo/php-todo.git'
      }
        }

        stage ('Build Docker Image') {
            steps {
                script {

                       sh "docker build -t hasmo/php:0.0.1${env.BRANCH_NAME}-${env.BUILD_NUMBER} ."
                }
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: '99f88c1d-3661-41c9-9599-79943736419c', usernameVariable: 'hasmo')]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                    sh "docker push hasmo/php:0.0.1${env.BRANCH_NAME}-${env.BUILD_NUMBER}"
                }
            }
        }

    }
}