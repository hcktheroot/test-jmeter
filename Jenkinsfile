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
        println 'Branch ' + env.GIT_BRANCH
        println 'Commit ' + env.GIT_COMMIT
        println 'Local Branch ' + env.GIT_LOCAL_BRANCH
        println 'GIT_PREVIOUS_COMMIT' + env.GIT_PREVIOUS_COMMIT
        println 'GIT_PREVIOUS_SUCCESSFUL_COMMIT ' + env.GIT_PREVIOUS_SUCCESSFUL_COMMIT
        println 'GIT_URL' + env.GIT_URL
        println 'GIT_URL_N' + env.GIT_URL_N
        println 'GIT_AUTHOR_NAME' + env.GIT_AUTHOR_NAME
        println 'GIT_AUTHOR_EMAIL' + env.GIT_AUTHOR_EMAIL
        println 'GIT_COMMITTER_NAME' + env.GIT_COMMITTER_NAME
        println 'GIT_COMMITTER_EMAIL' + env.GIT_COMMITTER_EMAIL
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
