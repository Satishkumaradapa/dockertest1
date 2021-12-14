pipeline {
    agent any
    stages { 

	    stage('Clone Repository') {
		  steps {
			sh 'rm -rf dockertest1 '
	                sh 'git clone --single-branch --branch master https://github.com/Satishkumaradapa/dockertest1.git'
		  }		
	    }

	    stage('Build Docker Image') {
		  steps {
	        sh 'cd /var/lib/jenkins/workspace/pipelinetest/dockertest1'
	        sh 'cp -r /var/lib/jenkins/workspace/pipelinetest/dockertest1/* /var/lib/jenkins/workspace/pipelinetest'
	        sh 'docker build -t 8074764785/pipelinetest:v$BUILD_NUMBER .'
		  }
	    }

	    stage('Push Image to Docker Hub') {
		  steps {
	        sh  'docker push 8074764785/pipelinetest:v$BUILD_NUMBER'
		  }		
        }

	    stage('Deploy to Docker Host') {
		  steps {
	        sh 'docker stop dockertest1'
	        sh 'docker run --rm -dit --name dockertest1 --hostname dockertest1 -p 8004:80 8074764785/pipelinetest:v$BUILD_NUMBER'
	        
		  }
		}

	    stage('Check Webapp Reachability') {
		  steps {
	        sh 'sleep 10s'
	           sh 'curl http://ec2-13-126-164-184.ap-south-1.compute.amazonaws.com:8004'
		  }
	    }
    }
    post {  
         always {  
             echo 'This will always run'  
         }  
         success {  
             echo 'This will run only if successful'  
         }  
         failure {  
             mail bcc: '', body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: 'nihaladapa23444@gmail.com', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: 'adapasatishkumar74@gmail.com', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "adapasatishkumar74@gmail.com";  
         }  
         unstable {  
             echo 'This will run only if the run was marked as unstable'  
         }  
         changed {  
             echo 'This will run only if the state of the Pipeline has changed'  
             echo 'For example, if the Pipeline was previously failing but is now successful'  
         }  
     }  
 }
}
