node {
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/hcktheroot/test-jmeter.git'
      def dockerHome = tool 'myDocker'
      env.PATH = "${dockerHome}/bin:${env.PATH}"
      sh 'chmod 777 /var/run/docker.sock'
      sh 'docker --version'
      sh 'docker build -t test1:1 .'
      sh 'docker images ls'
   }
}
