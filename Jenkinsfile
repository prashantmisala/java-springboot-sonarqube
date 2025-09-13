pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/prashantmisala/java-springboot-sonarqube.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh "mvn validate"
                sh "mvn compile"
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "mvn sonar:sonar"
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Upload to Artifactory') {
            steps {
              withJfrogEnv('jfrog'){
                 sh "mvn clean deploy"  
            }
        }
    }
  }
}    
