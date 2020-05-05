pipeline{
    agent none
    stages{
        
        stage('compile'){
            agent any
            steps{
                sh 'mvn compile'
            }
        }
        stage('test'){
            agent any
            steps{
                sh 'mvn test'
            }
        }
        stage('package'){
            agent any
            steps{
                sh 'mvn package'
            }
        }
        stage('deploy'){
            agent any
            steps{
                sh label: '', script: '''rm -rf dockerimg
mkdir dockerimg
cd dockerimg
cp /var/lib/jenkins/workspace/ContinuousDeploymentJob/gameoflife-web/target/gameoflife.war .
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
