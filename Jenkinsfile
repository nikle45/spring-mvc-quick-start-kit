pipeline{
  agent { label 'Jenkin-Agent' }
  tools{
    jdk 'Java17'
    maven 'Maven3'
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
  }
}
