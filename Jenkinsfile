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
	        sh 'cd :/var/lib/jenkins/workspace/pipelinetest/dockertest1'
	        sh 'cp -r :/var/lib/jenkins/workspace/pipelinetest/dockertest1/* :/var/lib/jenkins/workspace/pipelinetest'
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
}
