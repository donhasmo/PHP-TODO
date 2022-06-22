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

                       sh "docker build -t hasmo/php:${env.BRANCH_NAME}-${env.BUILD_NUMBER} ."
                }
            }
        }
        stage("Start the app") {
            steps {
                sh "docker-compose up -d"
            }
        }
        stage ('Test endpoint') {
            steps {
                script {

                     sh "sleep 10"
                     sh "curl -i 50.112.225.144:8000"
                }
            }
        }

        // stage ('Push Docker Image') {
        //     when { expression { response.status == 200 } }
        //     steps{
        //         script {
        //     sh "docker login -u ${env.username} -p ${env.password}"

        //     sh "docker push hasmo/php:${env.BRANCH_NAME}-${env.BUILD_NUMBER}"
        //     }
        //   }
        // }

        

        stage ("Remove images") {
            steps {
                sh "docker-compose down"
                sh "docker system prune -af"
            }
        }
    }
}
