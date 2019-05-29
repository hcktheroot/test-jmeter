pipeline
{
agent any
 
    environment {
        docker_registry = "github.com"
        docker_repo = "hcktheroot"
        target_environment="test"
        
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
            steps{
                 script{
                     sh 'mvn clean package docker:build docker:push -Dmaven.test.skip=true -Dmaven.wagon.http.ssl.insecure=true -Djavax.net.ssl.trustStore=/opt/trust.jks -Dmaven.wagon.http.ssl.allowall=true -Dmaven.wagon.http.ssl.ignore.validity.dates=true -Ddocker.push.registry=github.com -Ddocker.repo=hcktheroot'
                 
                 }
                    
                 }
        }
 
        stage('Functional Testing')
        {
             steps{
                 println 'Functional Testing'
            }
        }
 
   }
post {
        always {
            print 'Done !!'
        }
    }
}
 
