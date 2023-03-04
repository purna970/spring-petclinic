pipeline {
    agent { 'ubuntu' }
    stages {
        stage('vcs') {
            steps {
                git url: 'https://github.com/spring-projects/spring-petclinic.git',
                    branch: 'declarative'
            }
        }
        stage('package') {
            steps {
                sh 'mvn package'
            }
        }
    }
} 