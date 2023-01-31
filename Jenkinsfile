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
                script {
                    // Use Version postfix to determine if this is a snapshot or release version
                    def NexusRepo = Version.endsWith("SNAPSHOT") ? 'DansDevOpsLab-SNAPSHOT' : 'DansDevOpsLab-RELEASE'

                    nexusArtifactUploader artifacts: 
                    [[artifactId: "${ArtifactId}", 
                    classifier: '', 
                    file: "target/${ArtifactId}-${Version}.war", 
                    type: 'war']], 
                    credentialsId: 'b7cd8268-fff7-4247-88ad-71a53132e0b0', 
                    groupId: "${GroupId}", 
                    nexusUrl: '172.20.10.60:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: "${NexusRepo}", 
                    version: "${Version}"
                }
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

        // Stage 5 : Deploy to tomcat
        stage ('Deploy to tomcat') {
            steps {
                echo 'deploying....'
                sshPublisher(publishers: 
                [sshPublisherDesc(
                    configName: 'Ansible_Controller', 
                    transfers: [
                        sshTransfer(
                            cleanRemote: false, 
                            execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy.yaml -i /opt/playbooks/hosts', 
                            execTimeout: 120000
                            )], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false)
                    ])
            }
        }
                
        // Stage 6 : Deploy to docker
        stage ('Deploy to Docker') {
            steps {
                echo 'deploying....'
                sshPublisher(publishers: 
                [sshPublisherDesc(
                    configName: 'Ansible_Controller', 
                    transfers: [
                        sshTransfer(
                            cleanRemote: false, 
                            execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy_docker.yaml -i /opt/playbooks/hosts', 
                            execTimeout: 120000
                            )], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false)
                    ])
            }
        }
    }


}