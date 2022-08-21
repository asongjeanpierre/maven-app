node{
  stage('Checkout') {
     sh 'rm -rf maven-app'
     sh 'git clone https://github.com/asongjeanpierre/maven-app.git'
    }
   stage('Environment') {
      sh 'git --version'
      sh 'printenv'
    }
  stage('Build') {
     sh '''
     cd maven-app
     ls -al
     mvn test
     mvn install
     '''
   }
  stage('Upload to AWS') {
    sh 'cd /bitnami/jenkins/home/.m2/repository/com/justiceleague/justiceleague-tracker/0.0.1-SNAPSHOT'
     steps {
         withAWS(region:'us-east-1',credentials:'mayank') {
         sh 'echo "Uploading content with AWS creds"'
         s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'justiceleague-tracker-0.0.1-SNAPSHOT.jar', bucket:'santoshjenkins')
        }
     }
  }
  
  stage('Deployment'){
      sh 'java -jar -Dserver.port=8888 -Dspring.profiles.active=dev /bitnami/jenkins/home/.m2/repository/com/justiceleague/justiceleague-tracker/0.0.1-SNAPSHOT/justiceleague-tracker-0.0.1-SNAPSHOT.jar > /dev/null 2> /dev/null < /dev/null &'
  }
}
