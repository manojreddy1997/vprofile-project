pipeline{
    agent any 
        tools {
            maven "MAVEN3"
            jdk "OracleJDK17"
        }

        environment{

            SNAP_REPO = 'vprofile-snapshot'
            NEXUS_USER= 'admin'
            NEXUS_PASS= 'jenkins123'
            RELEASE_REPO = 'vprofile-release'
            CENTRAL_REPO = 'vprofile-cewntral'
            NEXUSIP= '172.31.23.68'
            NEXUS_GRP_REPO = 'vpro=maven-group'
            NEXUS_LOGIN='nexuislogin'

        }

        stages{
            stage('Build'){
                steps{
                    sh 'mvn -s settings.xml -Dskiptests install'
                }

            }
        }
    
}