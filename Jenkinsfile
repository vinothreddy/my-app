node{
   stage('CODE FROM GITHUB'){
     git 'https://github.com/vinothreddy/my-app.git'
   }
 
 
 
 stage('MAVEN PACKAGING'){

     def mvnHome =  tool name: 'maven', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb-0.0.5.war* target/newapp.war'
   }
   
   stage('SONARQUBE CODE QUALITY') {
	        def mvnHome =  tool name: 'maven', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
   
   stage('DOCKER IMAGE BUILD'){
   sh 'docker build -t vinothreddy/java-web-app .'
   }
   stage('DOCKER IMAGE PUSH'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u vinothreddy -p ${dockerPassword}"
    }
   sh 'docker push vinothreddy/java-web-app'
   }
   
   stage('NEXUS IMAGE PUSH'){
   sh "docker login -u admin -p admin 3.111.53.204:8083"
   sh "docker tag vinothreddy/java-web-app 3.111.53.204:8083/vinonex"
   sh 'docker push 3.111.53.204:8083/vinonex'
   }
   
   stage('REMOVE PREVIOUS CONTAINER'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   
   
 stage('DOCKER DEPLOYMENT'){
   sh 'docker run -d -p 8091:8080 --name tomcattest vinothreddy/java-web-app' 
   }
   
}
}
