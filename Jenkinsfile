node {
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/hcktheroot/test-jmeter.git'
      def dockerHome = tool 'myDocker'
      env.PATH = "${dockerHome}/bin:${env.PATH}"
      sh 'docker --version'
      sh 'docker build -t Var1:1 .'
      sh 'docker images ls'
   }
}
