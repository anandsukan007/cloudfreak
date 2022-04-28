pipeline {
  agent any
    tools {
      maven 'maven'
                 jdk 'JAVA_HOME'
    }
    stages {      
        /*stage('Build maven ') {
            steps { 
                    sh 'pwd'      
                    sh 'mvn  clean install package'
            }
        }*/
	    
	stage('build && SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonar6') {
                    // Optionally use a Maven environment you've configured already
                    withMaven(maven:'maven') {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }    
        
        stage('Copy Artifact') {
           steps { 
                   sh 'pwd'
		   sh 'cp -r target/*.jar docker'
           }
        }
         
        stage('Build docker image') {
           steps {
               script {         
                 def customImage = docker.build('anandsukan007/petclinic', "./docker")
                 docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                 customImage.push("${env.BUILD_NUMBER}")
                 }                     
           }
        }
	  }
    }
}
