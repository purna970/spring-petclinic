pipeline {
    agent { label 'ubuntu' }
    triggers { pollSCM('* * * * *') } 
    parameters { choice(name: 'maven', choices: ['install', 'clean', 'package', 'clean package'], description: 'maven packages') } 
    stages {
        stage('vcs') {
            steps {
                git url: 'https://github.com/purna970/spring-petclinic.git',
                    branch: 'declarative'
            }
        }
        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "ARTIFACTORY_SERVER",
                    url: 'https://poornaboda.jfrog.io/artifactory',
                    credentialsId: 'Meenakshi'
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: 'libs-release',
                    snapshotRepo: 'libs-snapshot'
                )

                rtMavenResolver (
                    id: "MAVEN_RESOLVER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: 'libs-release',
                    snapshotRepo: 'libs-snapshot'
                )
            }
        }

        stage ('Exec Maven') { 
            tools {
                jdk 'Java'
            }
            steps {
                rtMavenRun (
                    tool: 'Maven', // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER",
                    resolverId: "MAVEN_RESOLVER"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "ARTIFACTORY_SERVER"
                )
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