pipeline{

    agent any 

    tools{
        maven "Maven"
    }

    stages{
        stage("build & SonarQube analysis"){
            steps{
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn clean package sonar:sonar'
                }
            }
        }

        stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
        }

        stage("Maven Packaging"){
          steps{
           sh 'mvn package'
          }
        }

        stage("Build Image") {
          steps{
           sh 'docker build -t java-maven-app:$BUILD_NUMBER .'
          }
         }
    }
}