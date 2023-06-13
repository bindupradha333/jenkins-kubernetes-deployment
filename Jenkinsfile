pipeline {

  environment {
    dockerimagename = "bindupradha/react-app"
    dockerImage = ""
    env.PATH = "${/var/jenkins_home}/bin:${env.PATH}"
  }

  agent { dockerfile true }

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/bindupradha333/jenkins-kubernetes-deployment.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }
      }
    }

  }

}
