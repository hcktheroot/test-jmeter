pipeline {
  environment {
    registry = "hcktheroot/hck-jmeter-docker"
    registryCredential = 'docker-hub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        deleteDir() /* clean up our workspace */
        sh 'git config --global http.sslVerify false'
        git credentialsId: 'git-creds', url: 'https://github.com/hcktheroot/test-jmeter.git'
        println 'app ' + $GIT_BRANCH
        println 'bbb ' + $GIT_COMMIT
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
  post {
         always {
             print 'Done !!'
         }
     }
}
