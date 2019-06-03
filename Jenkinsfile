pipeline {
  environment {
    registry = "hcktheroot/hck-jmeter-docker"
    registryCredential = 'docker-hub'
    dockerImage = ''
    localbranch = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        script {
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
          localbranch = env.GIT_LOCAL_BRANCH
          println '$localbranch}'
          println 'apple - ' + env.GIT_LOCAL_BRANCH
          sh 'sed -i -e "s/JMXFILENAME/"' + env.GIT_LOCAL_BRANCH + '"/g" Dockerfile'
          sh 'cat Dockerfile'
          }
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":" + env.GIT_LOCAL_BRANCH
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
    ##dockerImage = docker.build registry + ":" + env.GIT_LOCAL_BRANCH + "_$BUILD_NUMBER"

    stage('Deploy to Kubernetes Cluster ')
    {
        steps{
            script{
               sh 'sed -i -e "s/BRANCH/"' + env.GIT_LOCAL_BRANCH + '"/g" *.yaml'
               sh 'cat jmeter-job.yaml'
               kubernetesDeploy(
               #kubeconfigId: 'kubeconfig-id',
               configs: 'deployment/*.yml',
               enableConfigSubstitution: true
                            )

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
