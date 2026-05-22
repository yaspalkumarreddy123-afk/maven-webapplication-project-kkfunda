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
	   git branch: 'dev', url: 'https://github.com/kkdevopsb8/maven-webapplication-project-kkfunda.git'
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
            --upload-file /var/lib/jenkins/workspace/MBPL-Pipeline/target/maven-web-application.war \
            "http://13.201.29.253:8080/manager/text/deploy?path=/maven-web-application&update=true"
            '''
	   }
	 }

	} //stages  ending

} //pipeline ending
