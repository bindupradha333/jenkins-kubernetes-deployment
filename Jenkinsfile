pipeline {

  environment {
    
	dockerimagename = "bindupradha/react-app"
        dockerImage = ""
    
  }

  agent any
  
    
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

    stage('Docker Push') {
    	agent any
      steps {
      	withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
        	sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh 'docker push bindupradha/react-app:latest'
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
echo env.dockerHubUser
echo "${env.dockerHubUser}"
echo "${dockerHubUser}"
echo env.dockerHubPassword
echo "${env.dockerHubPassword}"
echo "${dockerHubPassword}"
