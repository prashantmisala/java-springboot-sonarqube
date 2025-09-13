pipeline {
    agent any

    environment {
        // SonarQube server configured in Jenkins (Manage Jenkins > Configure System)
        SONARQUBE_ENV = 'MySonarQube'  

        // Artifactory server ID configured in Jenkins (Manage Jenkins > Configure System > JFrog)
        ARTIFACTORY_SERVER = 'MyArtifactory'  

        // Maven tool configured in Jenkins (Manage Jenkins > Global Tool Configuration)
        MAVEN_HOME = tool 'Maven3'
    }

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
