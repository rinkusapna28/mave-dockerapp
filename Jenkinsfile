pipeline {
    agent any
   stages{
    stage('SCM Checkout'){
         git credentialsId: '07b96208-9734-4475-8f43-f9fa98ff1fa7', url: 'https://github.com/ValaxyTech/hello-world.git'
      }
	stage('build package'){
         sh label: '', script: 'mvn clean install'
      } 
	stage('build Docker Image'){
         sh 'docker build -t srinku28/mavenappconainer:0.0.1 .'
      } 
	  ////////text password script userd here to login Docker with user name and password
	stage('Push Docker Image'){       
	   withCredentials([string(credentialsId: 'docker-pwd3', variable: 'dockerpasswd')]) {
         sh "docker login -u srinku28 -p ${dockerpasswd}"
      }
	     
         sh 'docker push srinku28/mavenappconainer:0.0.1'
      }
	  ////////ssh-agent plugin and credetials used here (jenkinsssh is credentials saved on jenkins)
    stage('Run container on Dev server'){
		 sshagent(['jenkinsssh']) {
		     sh 'ssh -o StrictHostKeyChecking=no root@172.16.4.74 apt-get update'
             sh 'ssh -o StrictHostKeyChecking=no root@172.16.4.74 apt-get install docker.io -y'			 
	         sh 'ssh -o StrictHostKeyChecking=no root@172.16.4.74 docker run -d -p 8086:8080 --name mavenappconainer srinku28/mavenappconainer:0.0.1'	 
    }
  } 	  
}
}
