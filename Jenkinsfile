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
                echo 'testing......'

            }
        }

        // Stage2 : Publish to Nexus
        stage ('Publish to Nexus'){
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'VinayDevOpsLab', classifier: '', file: 'target/VinayDevOpsLab-0.0.11-SNAPSHOT.war', type: 'war']], credentialsId: '907e0b19-23f7-4719-b2f2-90a4a1ddca58', groupId: 'com.vinaysdevopslab', nexusUrl: '172.20.10.233:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'BriFitz-DevOps-SNAPSHOT', version: '0.0.11-SNAPSHOT'
            }
        }

        // Stage3 : Deploying
        stage ('Deploy'){
            steps {
                echo 'deploying......'

            }
        }

        
        
    }

}