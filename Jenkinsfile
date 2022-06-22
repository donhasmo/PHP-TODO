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
        stage("Test endpoint") {
          steps {
              script {
                  while (true) {
                      def response = httpRequest 'http://localhost:8000'
                      if (response.status == 200) {
                          withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: '99f88c1d-3661-41c9-9599-79943736419c', usernameVariable: 'hasmo')]) {
                              sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                              sh "docker push hasmo/php:${env.BRANCH_NAME}-${env.BUILD_NUMBER}"
                          }
                          break 
                      }
                  }
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
