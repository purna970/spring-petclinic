pipeline {
    agent { label 'ubuntu' }
    triggers { pollSCM('* * * * *') } 
    parameters { choice(name: 'maven', choices: ['install', 'clean', 'package', 'clean package'], description: 'maven packages') } 
    tools {
            maven 'Maven' 
            jdk 'Java' 
        }
    stages {
        stage('vcs') {
            steps {
                git url: 'https://github.com/purna970/spring-petclinic.git',
                    branch: 'declarative'
            }
        }
        stage('package') {
            steps {
                sh "mvn package"
            }
        } 
        stage('archiveartifact') { 
            steps {
                archiveArtifacts artifacts: 'target/*.jar', 
                onlyIfSuccessful: true 
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
         stage('SonarQube analysis') {
            steps('sonarqube') {
             withSonarQubeEnv('sonar') {
             sh 'mvn clean verify sonar:sonar -Dsonar.login=6644b59884112fd89115612a4189a34a70d66b78 -Dsonar.organization=poorna -Dsonar.projectKey=poorna_my'
    } // submitted SonarQube taskId is automatically attached to the pipeline context
  }
         }
    }
} 