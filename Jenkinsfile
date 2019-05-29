node {
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/jglick/simple-maven-project-with-tests.git'
      def dockerHome = tool 'myDocker'
      env.PATH = "${dockerHome}/bin:${env.PATH}"
      sh 'sudo systemctl start docker'
      sh 'sudo systemctl enable docker'
      sh 'docker images ls'
   }
}
