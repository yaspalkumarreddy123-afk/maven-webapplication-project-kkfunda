node
{
	def mavenhome = tool name : "maven-3.9.13"
	stage('Check Out')
	{
		git branch: 'dev', url: 'https://github.com/yaspalkumarreddy123-afk/maven-webapplication-project-kkfunda.git'
	}
	stage('Compile')
	{
		sh "${mavenhome}/bin/mvn compile"
	}
	stage('Build')
	{
		sh "${mavenhome}/bin/mvn clean package"
	}
	stage('SonarQube Report')
	{
		sh "${mavenhome}/bin/mvn sonar:sonar"
	}
	stage('Deploy to Nexus')
	{
		sh "${mavenhome}/bin/mvn deploy"
	}
	stage('Deploy to TomCat') {
    withCredentials([usernamePassword(
        credentialsId: 'tomcat-credentials',
        usernameVariable: 'USERNAME',
        passwordVariable: 'PASSWORD'
    )]) {
        sh """
        curl -u $USERNAME:$PASSWORD \
        --upload-file target/maven-web-application.war \
        "http://13.233.140.22:8080/manager/text/deploy?path=/maven-web-application&update=true"
        """
    }
}
}
