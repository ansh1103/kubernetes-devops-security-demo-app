pipeline {
  agent any
  stages {
    stage('Build Artifact - Maven') {
      steps {
        sh "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }
    stage('Unit Tests - JUnit and Jacoco') {
      steps {
        sh "mvn test"
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
    }
    stage('Docker image build') {
      steps {
        withDockerRegistry([credentialsId: 'docker-hub-credential-stored-in-jenkins', url: ""]) {
          sh 'docker build -t docker-registry:5000/java-app:latest .'
          sh 'docker push docker-registry:5000/java-app:latest'
        }  
      }
    }
  }
}
