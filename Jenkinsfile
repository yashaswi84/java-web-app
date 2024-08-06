pipeline {
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    CI = true
    ARTIFACTORY_ACCESS_TOKEN = credentials('artifactory-access-token')
  }
  stages {
    stage('Build') {
      steps {
        script {
          // Run build command on the available node
          sh './mvnw clean install'
        }
      }
    }
    stage('Upload to Artifactory') {
      steps {
        script {
          // Run JFrog CLI commands using Docker directly
          sh '''
            docker run --rm \
            -v $(pwd):/mnt \
            -w /mnt \
            releases-docker.jfrog.io/jfrog/jfrog-cli-v2:2.2.0 \
            jfrog rt upload --url https://yashaswi.jfrog.io/artifactory/ \
            --access-token ${ARTIFACTORY_ACCESS_TOKEN} \
            target/demo-0.0.1-SNAPSHOT.jar java-web-app/
          '''
        }
      }
    }
  }
}
