pipeline {
  agent none
  stages {
      stage('Clone repository') {
      agent any
      steps {
        checkout scm
      }
    }    
    stage('Docker Build') {
      agent any
      steps {
        sh 'docker build -t vinaykumar94/apptest .'
      }
    }
    stage('Docker Push') {
      agent any
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh 'docker push vinaykumar94/apptest:latest'
        }
      }
    }
    stage('Docker Deploy') {
      agent any
      steps {
        sshagent(['ec2-docker']) {
                  sh 'docker container run --detach -p 8080:80 vinaykumar94/apptest'
              }
      }
    }
  }
}
