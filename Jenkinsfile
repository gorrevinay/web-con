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
        withCredentials([sshUserPrivateKey(credentialsId: 'ec2-docker', keyFileVariable: 'keyid', usernameVariable: 'userid')]) {
                  sh 'ssh -tt -i  $keyid $userid@ec2-3-111-39-4.ap-south-1.compute.amazonaws.com'
                  sh 'docker container run --detach -p 80:80 vinaykumar94/apptest'
                }
              }
      }
    }
  }

