pipeline
{
agent any



  stages
   {
        stage('Checkout')
        {
            steps{
                deleteDir() /* clean up our workspace */
                sh 'git config --global http.sslVerify false'
                git branch: 'master', url: 'https://github.com/hcktheroot/test-jmeter.git'
                sh 'ls -la'
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
