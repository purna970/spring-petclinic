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
         stage('copy') {
            steps {
                sh "mkdir -p /tmp/archive/${JOB_NAME}/${BUILD_ID} && cp ./target/*.jar  /tmp/archive/${JOB_NAME}/${BUILD_ID}" 
                sh "aws s3 sync /tmp/archive/${JOB_NAME}/${BUILD_ID} s3://praneethpoorna/ "
            }
        }
        stage('archiveartifact') { 
            steps {
                archiveArtifacts artifacts: 'target/*.jar', 
                onlyIfSuccessful: true 
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
    }
} 