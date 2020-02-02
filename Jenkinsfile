<tomcat-users>
    <user username="admin" password="password" roles="manager-gui,admin-gui,manager-script"/>
</tomcat-users>

/var/lib/tomcat8/webapps/manager/META-INF/context.xml
<Context antiResourceLocking="false" privileged="true" >
 <!-- <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->

I got it working, I needed to do two things:
1) I had to use -o StrictHostKeyChecking=no:
scp -v -o StrictHostKeyChecking=no my_file ubuntu@my_address:~/MyProject
instead of:
scp my_file ubuntu@my_address:~/MyProject
2) I needed to copy my id_rsa to /var/lib/jenkins/.ssh
The /var/lib/jenkins/.ssh folder and files inside of it need to be owned by jenkins.
--------------------------------------------------------------------
node() {
 stage('ContinuousDownload')
 {
     git 'https://github.com/selenium-saikrishna/maven.git'
 }
 stage('ContinuousBuild')
 {
     sh '/opt/apache-maven/bin/mvn package'
 }
 stage('ContinuousDeployment')
 {
     sh 'scp -v -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/pipeline/webapp/target/webapp.war ec2-user@172.31.28.31:/var/lib/tomcat8/webapps/testwebapp.war'
 }
 stage('ContinuousTesting')
 {
     git 'https://github.com/selenium-saikrishna/FunctionalTesting.git'
     sh label: '', script: 'echo "TestingPassed"'
 }
 stage('ContinuousDelivery')
 {
     sh 'scp -v -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/pipeline/webapp/target/webapp.war ec2-user@172.31.19.158:/var/lib/tomcat8/webapps/testwebapp.war'
 }    
}
###########################Final Pipeline########################
node() {
 stage('ContinuousDownload')
 {
     git 'https://github.com/selenium-saikrishna/maven.git'
 }
 stage('ContinuousBuild')
 {
     sh label: '', script: '/opt/apache-maven/bin/mvn package'
 }
 stage('ContinuousDeployment')
 {
     sh label: '', script: 'scp -v -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/pipeline/webapp/target/webapp.war ec2-user@172.31.28.31:/var/lib/tomcat8/webapps/testwebapp.war'
 }
 stage('ContinuousTesting')
 {
     git 'https://github.com/selenium-saikrishna/FunctionalTesting.git'
     sh label: '', script: 'echo "TestingPassed"'
 }
 stage('ContinuousDelivery')
 {
     sh label: '', script: 'scp -v -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/pipeline/webapp/target/webapp.war ec2-user@172.31.19.158:/var/lib/tomcat8/webapps/prodwebapp.war'
 }
}
######################################################################################
node('master') {
 stage('ContinuousDownload')
 {
     git 'https://github.com/bhupendra1988/maven-project.git'
 }
 stage('ContinuousBuild')
 {
     sh label: '', script: '/opt/apache-maven/bin/mvn package'
 }
 stage('ContinuousDeployment')
 {
     sh label: '', script: 'scp -v -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/pipeline/webapp/target/webapp.war ec2-user@172.31.28.31:/var/lib/tomcat8/webapps/testwebapp.war'
 }
 stage('ContinuousTesting')
 {
     git 'https://github.com/selenium-saikrishna/FunctionalTesting.git'
     sh label: '', script: 'echo "TestingPassed"'
 }
 stage('ContinuousDelivery')
 {
     sh label: '', script: 'scp -v -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/pipeline/webapp/target/webapp.war ec2-user@172.31.19.158:/var/lib/tomcat8/webapps/prodwebapp.war'
 }
}
#######################################
