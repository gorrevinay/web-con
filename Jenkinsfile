pipeline {
  agent none
  stages {
    stage('Docker Build') {
      agent any
      steps {
        sh 'docker build -t vinaykumar94/apptest .'
      }
    }
    stage('Docker Push') {
      agent any
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh 'docker push vinaykumar94/apptest:latest'
        }
      }
    }
    stage('Docker Deploy') {
      agent any
      steps {
        sh 'docker container run --detach -p 80:80 vinaykumar94/apptest'
      }
    }
  }
}
