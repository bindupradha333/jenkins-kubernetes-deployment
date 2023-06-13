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
	environment {
    docker_username = "bindupradha"
    docker_password = "dckr_pat__gmjmLfwYmSqX5h22Cs045Kx0Cs"
	
    
  }
    stage('Login-Into-Docker') {
      steps {
        container('docker') {
          sh 'docker login -u <docker_username> -p <docker_password>'
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
  }
}
