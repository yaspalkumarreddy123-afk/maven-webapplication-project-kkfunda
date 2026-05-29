node {
    def mavenhome = tool name: "maven-3.9.13"

    // 🔔 Notify start
    notifyBuild('STARTED')

    try {
        echo "git branch Name: ${env.BRANCH_NAME}"
        echo "build number: ${env.BUILD_NUMBER}"

        stage('Check Out') {
            git branch: 'dev', url: 'https://github.com/yaspalkumarreddy123-afk/maven-webapplication-project-kkfunda.git'
        }

        stage('Compile') {
            sh "${mavenhome}/bin/mvn compile"
        }

        stage('Build') {
            sh "${mavenhome}/bin/mvn clean package"
        }

        stage('SonarQube Report') {
            sh "${mavenhome}/bin/mvn sonar:sonar"
        }

        stage('Deploy to Nexus') {
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
                "http://13.201.0.83:8080/manager/text/deploy?path=/maven-web-application&update=true"
                """
            }
        }

        // 🔔 Notify success
        notifyBuild('SUCCESS')

    } catch (err) {
        // 🔔 Notify failure
        notifyBuild('FAILURE')
        throw err
    }
}
def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#278EF5'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary, channel: '#pipeline-project')
   
}
