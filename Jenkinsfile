pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: react-app
            image: bravinwasike/react-app:latest
            command:
            - cat
            tty: true
          - name: docker
            image: docker:latest
            command:
            - cat
            tty: true
            volumeMounts:
             - mountPath: /var/run/docker.sock
               name: docker-sock
          volumes:
          - name: docker-sock
            hostPath:
              path: /var/run/docker.sock    
        '''
    }
  }
  stages {
    stage('Clone') {
      steps {
        container('react-app') {
          git branch: 'main', changelog: false, poll: false, url: 'https://mohdsabir-cloudside@bitbucket.org/mohdsabir-cloudside/java-app.git'
        }
      }
    }  
    
    stage('Build-Docker-Image') {
      steps {
        container('docker') {
          sh 'docker build -t bravinwasike/react-app:latest .'
        }
      }
    }
	
    stage('Login-Into-Docker') {
      steps {
        container('docker') {
          sh 'docker login -u bindupradha -p dckr_pat__gmjmLfwYmSqX5h22Cs045Kx0Cs'
      }
    }
    }
     stage('Push-Images-Docker-to-DockerHub') {
      steps {
        container('docker') {
          sh 'docker push bravinwasike/react-app:latest'
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
    
}

