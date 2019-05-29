pipeline
{
   agent none


   environment {
       docker_registry = "docker.com"
       docker_repo = "hck-jmeter"
       target_environment="test-apps"

   }

  options {
    timestamps()
  }

 stages
  {
       stage('Checkout')
       {
           steps{
               deleteDir() /* clean up our workspace */
               sh 'git config --global http.sslVerify false'
               git branch: 'master', credentialsId: 'git-creds', url: 'https://github.com/hcktheroot/test-jmeter.git'
           }
       }

       stage('SonarQube Analysis')
       {
            steps{
              println 'Sonar Reporting'
           }
       }

       stage('Build & Dockerize')
       {
           agent { docker 'M3'}
           steps{
                println 'hello Maven'
                sh 'mvn --version'
                sh 'mvn clean package docker:build docker:push -Dmaven.test.skip=true -Dmaven.wagon.http.ssl.insecure=true -Djavax.net.ssl.trustStore=/opt/trust.jks -Dmaven.wagon.http.ssl.allowall=true -Dmaven.wagon.http.ssl.ignore.validity.dates=true -Ddocker.push.registry=docker.com -Ddocker.repo=hcktheroot/test-jmeter'
              }
       }

       stage('Functional Testing')
       {
            steps{
                println 'Functional Testing'
           }
       }

       stage('Security Scanning')
       {
            steps{
             println 'Scanning !!'
           }
       }
  }
post {
       always {
           print 'Done !!'
       }
   }
}
