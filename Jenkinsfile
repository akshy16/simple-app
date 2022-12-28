pipeline {
    agent any
    tools {
        maven 'MAVEN_HOME'
    }
    options {
        buildDiscarder logRotator(daysToKeepStr: '5', numToKeepStr: '7')
    }
    stages{
        stage('Build'){
            steps{
                  bat 'mvn clean compile'
                 archiveArtifacts artifacts: 'target/*.war*', onlyIfSuccessful: true
            }
        }
        stage('Upload War To Nexus'){
            steps{
                script{

                    def mavenPom = readMavenPom file: 'pom.xml'
                    def nexusRepoName = mavenPom.version.endsWith("SNAPSHOT") ? "simpleapp-snapshot" : "simpleapp-release"
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'simple-app', 
                            classifier: '', 
                            file: "target/simple-app-${mavenPom.version}.war", 
                            type: 'war'
                        ]
                    ], 
                    credentialsId: 'nexus3', 
                    groupId: 'in.javahome', 
                    nexusUrl: 'http://localhost:8081/', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'simple-app', 
                    version: "${mavenPom.version}"
                    }
            }
        }
    }
}
