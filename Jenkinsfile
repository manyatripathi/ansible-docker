pipeline 
{

	tools
	{
		maven 'MAVEN_HOME'
		jdk 'JAVA_HOME'
	}
	options
	{
		buildDiscarder(logRotator(numToKeepStr: '3', artifactNumToKeepStr: '3'))
		
	}
	agent 
	{
		node 
		{	
			label ''
		}
	}
	stages
	{
		
		stage('code checkout')
		{
			steps
			{
				script
				{
					properties([pipelineTriggers([githubPush()])])
				}
				git 'https://github.com/manyatripathi/my-app'
				sh "mvn clean"
			}
	  
		}
		stage('compile')
		{
			
		  	steps
			{
				sh "mvn compile"
			}
			
	  
		}
		stage('unit test')
		{
			steps
			{
				sh "mvn test"
				junit 'target/surefire-reports/*.xml'
				
			}
		}
		stage('code coverage')
		{
			steps
			{
				sh 'mvn package'
				
			}
		}
		stage('sonarqube analysis')
		{
			
			steps
			{
				withSonarQubeEnv('sonar-1')
				{
					sh 'mvn sonar:sonar -Dsonar.host.url="http://localhost:9000" '			  
				}
				timeout(time: 10, unit: 'MINUTES') 
				{
					waitForQualityGate abortPipeline: true
				}
			}
		}
		stage('war')
		{
			steps
			{
				sh "mvn war:war"
			}
		}
		stage('deploy to artifactory')
		{
			steps
			{
				sh "mvn deploy"
			}
		}/*
		stage ('deploy to tomcat')
		{
			steps
			{
				sh "cp target/myweb-0.0.5.war /opt/tomcat/apache-tomcat-9.0.20/webapps"
			}
		}*/
		/*
		stage('integration testing')
		{
			steps
			{ 
				sh "mvn integration-test"
			}
		}
		*/
	}
  
  	/*post 
	{
	    	always 
		{
			mail to: 'manyatripathi02@gmail.com',
			subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
			body: "${env.BUILD_URL} has result ${currentBuild.result} at stage ${FAILED_STAGE}"
	    	}
	}*/
  
}
