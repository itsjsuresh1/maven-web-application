node{

def mavenHome = tool name: "maven3.8.5"
  
  echo "The Node name is: ${env.NODE_NAME}"
  echo "The Job name is: ${env.JOB_NAME}"
  echo "The build number is :${env.BUILD_NUMBER}"

// check out stage
stage('CheckoutCode')
{

git branch: 'development', credentialsId: 'e950ec92-b4c2-47ee-bda7-3c343e9403bc', url: 'https://github.com/itsjsuresh1/maven-web-application.git'

}

// build stage
stage ('Build')
{

sh "$mavenHome/bin/mvn clean package"

}

// Generate SonarQube report

stage('SonarQubeReport'){

sh "$mavenHome/bin/mvn sonar:sonar"
}

//Upload artifact into artifactory repository


stage('Nexusrepository'){

sh "$mavenHome/bin/mvn deploy"
}


//Deploy application into tomcat server

stage('DeployApptoTomcat'){

sshagent(['42086cf1-4c5a-49f1-99ea-01803cbca663']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.91.226:/opt/apache-tomcat-9.0.70/webapps"

}


}


} // node closing
