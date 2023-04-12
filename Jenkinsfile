pipeline {
    agent any
	
	  tools
    {
       maven "Maven"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/devops4solutions/CI-CD-using-Docker.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t samplewebapp:latest .' 
                sh 'docker tag samplewebapp tirusoma/samplewebapp:latest'
                //sh 'docker tag samplewebapp tirusoma/samplewebapp:$BUILD_NUMBER'
               
          }
        }
     
 stage('Push Docker Image') {
            steps{             
               withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
		            sh "docker login -u tirusoma -p ${dockerHubPwd}"
				 }				
				    sh 'docker tag samplewebapp tirusoma/samplewebapp:latest'
                    sh 'docker push tirusoma/samplewebapp:latest'	            
            }
	    }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
                sh "docker run -d -p 8003:8080 tirusoma/samplewebapp:latest"
 
            }
        }
 stage('Run Docker container on remote hosts') {
             
            steps {
                sh "docker -H ssh://jenkins@10.0.10.111 run -d -p 8003:8080 tirusoma/samplewebapp:latest"
 
            }
        }
    }
	}
    
