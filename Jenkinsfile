pipeline{
  agent { label 'Jenkin-Agent' }
  tools{
    jdk 'Java17'
    maven 'Maven3'
  }
  environment{
    APP_NAME = "START-KIT"
    RELEASE = "1.0.0"
    DOCKER_USER = "ghalge"
    DOCKER_PASS = 'dockerhub'
    IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
    IMAGE_TAG = "${RELEASE}-${BUILDNUMBER}"
  }
  stages{
    stage("Checking out from SCM"){
      steps{
        git changelog: false, credentialsId: 'github', poll: false, url: 'https://github.com/nikle45/spring-mvc-quick-start-kit'
      }
    }

    stage("Bulid Application"){
      steps{
        sh 'mvn clean package'
      }
    }
    stage("Test"){
      steps{
        sh 'mvn test'
      }
    }
    stage("sonarqube Analysis"){
      steps{
        script{
        withSonarQubeEnv(credentialsId: 'Jenkins-sonarqube-token') {
          sh 'mvn sonar:sonar'
          }
        }   
      }
    }
    stage("Quality Gate"){
      steps{
        script{
          waitForQualityGate abortPipeline: false, credentialsId: 'Jenkins-sonarqube-token'
        }
      }
    }
    stage("Build and push docker image "){
      steps{
        script{
          docker.withRegistry('',DOCKER_PASS){
            docker_image = docker.build "${IMAGE_NAME}"
          }
          docker.withRegistry('',DOCKER_PASS){
            docker_image.push("${IMAGE_TAG}")
            docker_image.push('latest')
          }
        }
      }
    }
  }
}
