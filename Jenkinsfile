node {
  withMaven(maven:'localMaven') {
    stage('Checkout') {
      checkout scm
    }

    stage('Build') {
      sh 'mvn clean install'
      def pom = readMavenPom file:'pom.xml'
      print pom.version
      env.version = pom.version
      currentBuild.description = "Release: ${env.version}"
    }

    stage('Image') {
      docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
        def app = docker.build "xisana/discovery-service:${env.version}"
        app.push()
      }
    }
  }
}