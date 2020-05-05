pipeline{
  agent none
  stages{
    stage('Compile'){
      agent any
      steps{
        sh 'mvn compile'
      }       
    }
    stage('Code Quality'){
      agent any
      steps{
        sh 'echo Sonarqube Code Quality Check Done'
      }
    }
    stage('Test'){
      agent any
      steps{
        sh 'mvn test'
      }
    } 
    stage('Package'){
      agent any
      steps{
        sh 'mvn package'
      }
    }
    stage('Upload War File To Artifactory'){
      agent any
      steps{
        sh 'echo Uploaded War file to Artifactory'
      }
    }
    stage('Deploy'){
      agent any
      steps{
        sh label: '', script: '''rm -rf dockerimg
mkdir dockerimg
cd dockerimg
cp /var/lib/jenkins/workspace/JenkinsfileJob/gameoflife-web/target/gameoflife.war .
touch dockerfile
cat <<EOT>>dockerfile
FROM tomcat
ADD gameoflife.war /usr/local/tomcat/webapps/
CMD ["catalina.sh", "run"]
EXPOSE 8080
EOT
sudo docker build -t webimage:$BUILD_NUMBER .
sudo docker container run -itd --name webserver$BUILD_NUMBER -p 8080 webimage:$BUILD_NUMBER'''
      }
    }
  }
}
