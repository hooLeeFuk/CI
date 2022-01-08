pipeline {
      tools {
        maven "maven"
       
    }
    
    environment {
    registry = "raddane90/timesheet"
    registryCredential = 'dockerHub'
    dockerImage = ''
    }
    agent any

    stages {
       stage ('Cloning Project From Git') {
            steps {
                 git branch: "radhouankhouadja",
                     url: "https://github.com/mohamed-kbaier/Timesheet",
                     credentialsId: "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";
            }
       }
       stage("Build") {
            steps {
                sh "mvn compile"
            }
       }
       stage("Unit Test"){
            steps{
                sh "mvn test"
            }
       }
  
       stage("Clean And Packaging")
       {
            steps {
                sh "mvn clean package"
            }
       }
      
       stage('Building our image') {
            steps{
                 script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                 }
            }
       }
       stage('Deploy our image') {
             steps {
                  script {
                     docker.withRegistry( '', registryCredential ) {
                     dockerImage.push()
                      }
                  }
            }
       }
       stage('Cleaning up') {
             steps {
                   sh "docker rmi $registry:$BUILD_NUMBER"
                    }
            }
       stage('Pulling MySQL') {
             steps {
                    sh "docker pull mysql"
                    }
             }
       stage('Pulling Project') {
             steps {
                    sh "docker pull raddane90/timesheet:14"
                    }
             }
    }
       post{
            always{
                 emailext body: 'New update comming', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Timesheet'
                 cleanWs()
                 }
            }
}
