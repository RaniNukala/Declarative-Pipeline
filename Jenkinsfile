pipeline {
  agent any
  stages {
    stage('Continuous Download') {
      steps {
        // Source code address of maven repository
        git branch: 'main', url: 'https://github.com/sysgeeks4u/Maven-Tomcat.git'
      }
    }

    stage('Continuous Build') {
      steps {
		  script {
       		try {
			  //main code that might fail 
	          // Building executable application
        	  sh 'mvn package'
           }
           catch (Exception e) {
			  //handles the error 
              echo "Build failed…"
			  //Error email notification to Build Team
			  mail bcc: '', body: 'CI CD & CD Process, Maven Build has been failed', cc: 'raninukala2221@gmail.com', from: '', replyTo: '', subject: 'Build Failed', to: 'sangavirani321@gmail.com'
           }
           finally {
              // always runs whether it success or failure of try block
              echo "cleaning up workspace"
           }
		   // Building executable application
           sh 'mvn package'
		}
      }
    }

    stage('Continuous Delivery') {
      steps {
        // To deliver application on a QA Server
        deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'admin-test', path: '', url: 'http://10.10.10.253:8080/')], contextPath: 'testapp', war: '**/*.war'
      }
    }
    
    stage('Continuous Testing') {
      steps {
        // Testing application by using testing.jar file
        git branch: 'main', url: 'https://github.com/sysgeeks4u/Functional-Testing.git'
        
        // To run jar file
        sh 'java -jar /var/lib/jenkins/workspace/Declarative-Pipeline/testing.jar'
      }
    }

    stage('Continuous Deploy') {
      steps {
        // Application deploying on live servers
        deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'prod-admin', path: '', url: 'http://10.10.10.247:8080/')], contextPath: 'prodapp', war: '**/*.war'
      }
    }
  }

}
