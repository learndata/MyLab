pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }

    environment {
        ArtifactId = readMavenPom().getArtifactId()
        Version = readMavenPom().getVersion()
        Name = readMavenPom().getName()
        GroupId = readMavenPom().getGroupId()
    }

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }

        // // Stage3 : Publish the source code to Sonarqube
        // stage ('Sonarqube Analysis'){
        //     steps {
        //         echo ' Source code published to Sonarqube for SCA......'
        //         withSonarQubeEnv('sonarqube'){ // You can override the credential to be used
        //              sh 'mvn sonar:sonar'
        //         }

        //     }
        // }

        // Stage 3 - publish to nexus
        stage ('Publish to Nexus') {
            steps {
                nexusArtifactUploader artifacts: 
                [[artifactId: '${ArtifactId}', 
                classifier: '', 
                file: 'target/DansDevOpsLab-0.0.5-SNAPSHOT.war', 
                type: 'war']], 
                credentialsId: 'b7cd8268-fff7-4247-88ad-71a53132e0b0', 
                groupId: '${GroupId}', 
                nexusUrl: '172.20.10.60:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'DansDevOpsLab-SNAPSHOT', 
                version: '${Version}'
            }
        }

        // Stage 4 - print info
        stage ('Print Environment Variable') {
            steps {
                echo "Artifact Id is '${ArtifactId}'"
                echo "Version is '${Version}'"
                echo "GroupID is '${GroupId}'"
                echo "Name is '${Name}'"
            }
        }

        stage ('Deploy') {
            steps {
                echo 'deploying....'
            }
        }
        
    }

}