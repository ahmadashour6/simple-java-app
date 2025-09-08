pipeline {
  agent any

  stages {
    stage('build') {
      steps {
        sh 'docker build -t java-app .'
      }
    }

    stage('push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'Password', usernameVariable: 'Username')]) {
          sh 'docker login --username $Username --password $Password'
          sh 'docker tag java-app ashour66/java-app'
          sh 'docker push ashour66/java-app'
        }
      }
    }

    stage('deploy') {
      steps {
        withAWS(credentials: 'aws-ci', region: 'us-east-1') {
          sh 'aws eks update-kubeconfig --region us-east-1 --name ahmad-sami'
          sh 'kubectl apply -f ./k8s/deployment.yaml'
        }
      }
    }
  }
}
