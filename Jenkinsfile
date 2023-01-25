pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
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

        stage ('Publish to Nexus') {
            steps {
                 nexusArtifactUploader artifacts: [[artifactId: 'VinayDevOpsLab', classifier: '', file: 'target/VinayDevOpsLab-0.0.4-SNAPSHOT.war', type: 'war']], credentialsId: 'df8850ac-7c3c-4384-85ec-5d10c09debe3', groupId: 'com.vinaysdevopslab', nexusUrl: '172.20.10.60:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'DansDevOpsLab-SNAPSHOT', version: '0.0.4-SNAPSHOT'
            }
        }

        stage ('Deploy') {
            steps {
                echo 'deploying....'
            }
        }
        
    }

}