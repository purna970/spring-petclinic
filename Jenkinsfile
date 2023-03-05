pipeline {
    agent { label 'ubuntu' }
    triggers { pollSCM('* * * * *') } 
    parameters { choice(name: 'maven', choices: ['install', 'package', 'clean package'], description: 'maven packages') } 
    tools {
        maven 'Maven' 
        JDK 'Java' 
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
                sh "mvn ${params.package}"
            }
        }
        stage {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', 
                onlyIfSuccessful: true 
                junit testResults: '**/surefire-reports/TEST-*>xml'
            }
        }
    }
} 