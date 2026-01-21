pipeline {
  agent any
  tools {
    jdk 'Java 17'
    maven 'Maven-home'
  }
  stages {
    stage('Checkout Code') {
      steps {
        echo 'Pulling from Github'
        git branch: 'main', credentialsId: 'Git-cred', url: 'https://github.com/Tpawar25/k8Testjenkin.git'
      }
    }
    stage('Test Code') {
      steps {
        echo 'JUNIT Test case execution started'
        bat 'mvn clean test'
        
      }
      post {
        always {
		  junit '**/target/surefire-reports/*.xml'
          echo 'Test Run is SUCCESSFUL!'
        }

      }
    }
    stage('Build Project') {
      steps {
        echo 'Building Java project'
        bat 'mvn clean package -DskipTests'
      }
    }
    stage('Build the Docker Image') {
      steps {
        echo 'Building Docker Image'
        bat 'docker build -t myindiaproj:1.0 .'
      }
    }
    stage('Push Docker Image to DockerHub') {
      steps {
        echo 'Pushing  Docker Image'
        withCredentials([string(credentialsId: 'docker-cred1', variable: 'DOCKER_PASS')]) {
  	      bat '''
          echo %DOCKER_PASS% | docker login -u Tpawa02 --password-stdin
          docker tag myindiaproj:1.0 Tpawa02/myindiaproj:1.0
          docker push Tpawa02/myindiaproj:1.0
          '''}
      }
    }
 }
  post {
    success {
      echo 'BUild and Run is SUCCESSFUL!'
    }
    failure {
      echo 'OOPS!!! Failure.'
    }
  }
}
