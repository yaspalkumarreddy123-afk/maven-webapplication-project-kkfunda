pipeline
{
	agent any
	tools
	{
	  maven "maven-3.9.13"
	}
	stages
	{
	 stage('checkout')
	 {
	    steps
	    {
	   git branch: 'dev', url: 'https://github.com/yaspalkumarreddy123-afk/maven-webapplication-project-kkfunda.git'
	   }
	 }
	 stage('Build')
	 {
	   steps
	   {
	      sh "mvn clean package"
	   }
	 }
	 stage('SQ REPORT')
	 {
	   steps
	   {
	   sh "mvn sonar:sonar"
	   }
	 }
	 stage('Upload to nexus')
	 {
	     steps
	     {
	    sh "mvn deploy"
	     }
	 }
	 stage('Deploy to tomcat')
	 {
	   steps
	   {
	      sh '''
            curl -u kk:password \
            --upload-file /var/lib/jenkins/workspace/MBPL-Pipeline_f9/target/maven-web-application.war \
            "http://43.205.195.137:8080/manager/text/deploy?path=/maven-web-application&update=true"
            '''
	   }
	 }

	} //stages  ending

} //pipeline ending
