pipeline
{
   agent any

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

       stage('Docker Build')
       {
           steps{
                script{
                def docker_version = 'myDocker'
                withEnv( ["PATH+DOCKER=${tool docker_version}/bin"] ) {
                    sh 'docker --version'
                    sh 'docker build -t hck-jmeter:3 .'
                    sh 'docker push hcktheroot/hck-jmeter:3'
                    }
                }

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
