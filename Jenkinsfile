pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }

    environment {
        artifactId = readMavenPom().getArtifactId()
        version = readMavenPom().getVersion()
        name = readMavenPom().getName()
        groupId = readMavenPom().getGroupId()
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
                echo 'testing......'

            }
        }

        // Stage3 : Publish to Nexus
        stage ('Publish to Nexus'){
            steps {
                script {
                    def NexusRepo = version.endsWith("SNAPSHOT") ? "BriFitz-DevOps-SNAPSHOT" : "BriFitz-DevOps-RELEASE"
                    nexusArtifactUploader artifacts:
                    [[artifactId: "${artifactId}",
                    classifier: '',
                    file: "target/${artifactId}-${version}.war",
                    type: 'war']],
                    credentialsId: '907e0b19-23f7-4719-b2f2-90a4a1ddca58',
                    groupId: "${groupId}",
                    nexusUrl: '172.20.10.233:8081',
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    repository: "${NexusRepo}",
                    version: "${version}"
                }
            }
        }

        // Stage4 : Print info
        stage ('Print environment variables'){
            steps {
                echo "artifactId is '${artifactId}'"
                echo "version is '${version}'"
                echo "name is '${name}'"
                echo "groupId is '${groupId}'"
            }
        }

        
        // Stage5 : Deploying the build artifact to Apache Tomcat
        stage ('Deploy to Tomcat'){
            steps {
                echo "Deploying ...."
                sshPublisher(publishers: 
                [sshPublisherDesc(
                    configName: 'Ansible_Controller', 
                    transfers: [
                        sshTransfer(
                                cleanRemote:false,
                                execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy.yaml -i /opt/playbooks/hosts',
                                execTimeout: 120000
                        )
                    ], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false)
                    ])
            
            }
        }
        

        // Stage6 : Deploying
        stage ('Deploy'){
            steps {
                echo 'deploying......'

            }
        }
        
    }

}