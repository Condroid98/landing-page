pipeline {  
  agent any 
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))   
  }
  stages {
    stage('Build Image') {
        steps {
            script {
                if (env.BRANCH_NAME == 'staging') {
            sh 'docker build -t hiro99/landing:0.0.$BUILD_NUMBER-stg .'
                }
                else if (env.BRANCH_NAME == 'production') {
            sh 'docker build -t hiro99/landing:0.0.$BUILD_NUMBER-prod .'   
               }
                else {
                    sh 'echo Nothing to Build'
                }
            }
        }
    }
    stage('Push to Registry') {
        steps {
            script {
                if (env.BRANCH_NAME == 'staging') {
            sh 'docker push hiro99/landing:0.0.$BUILD_NUMBER-stg' 
                }
                else if (env.BRANCH_NAME == 'production') {
            sh 'docker push hiro99/landing:0.0.$BUILD_NUMBER-prod'
               }
                else {
                    sh 'echo Nothing to Build'
                }

        }
      }
    } 
}
        post {
            success {
                slackSend channel: '#testbot',
                color: 'good',
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}"
            }    

            failure {
                slackSend channel: '#testbot',
                color: 'danger',
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}"
                }

        }
}
