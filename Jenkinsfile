pipeline {
    agent any 
    stages {
        stage('SCM Checkout') { 
            steps {
                git credentialsId: '07b96208-9734-4475-8f43-f9fa98ff1fa7', url: 'https://github.com/rinkusapna28/mave-dockerapp.git'
            }
        }
		stage('build package') { 
            steps {
                sh label: '', script: 'mvn clean install'
            }
        }
		stage('build Docker Image') { 
            steps {
                sh 'docker build -t srinku28/mavenappconainer:0.0.2 .'
            }
        }
		stage('Push Docker Image') { 
            steps {
                withCredentials([string(credentialsId: 'docker-pwd3', variable: 'dockerpasswd')]) {
         sh "docker login -u srinku28 -p ${dockerpasswd}"
		 sh 'docker push srinku28/mavenappconainer:0.0.2'
            }
		  }	
        }
		stage('Run container on Dev server') { 
            steps {
                sshagent(['jenkinsssh']) {
		     sh 'ssh -o StrictHostKeyChecking=no root@172.16.4.149 apt-get update'
             sh 'ssh -o StrictHostKeyChecking=no root@172.16.4.149 apt-get install docker.io -y'			 
	         sh 'ssh -o StrictHostKeyChecking=no root@172.16.4.149 docker run -it --rm -d -p 8087:8080 --name mavendeclerative srinku28/mavenappconainer:0.0.2'
            }
          }
        }
	}
}
