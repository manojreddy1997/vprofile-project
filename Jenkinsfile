def COLOR_MAP = [
    'SUCCESS': 'good', 
    'FAILURE': 'danger',
]

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
            CENTRAL_REPO = 'vpro-maven-central'
            NEXUSIP= '172.31.23.68'
            NEXUSPORT='8081'
            NEXUS_GRP_REPO = 'vpro-maven-group'
            NEXUS_LOGIN='nexuslogin'
            SONARSERVER='sonarserver'
            SONARSCANNER='sonarscanner'

        }

        stages{
            stage('Build'){
                steps{
                    sh 'mvn -s settings.xml -Dskiptests install'
                }

                post {
                    success {
                        echo 'now archiving'
                        archiveArtifacts artifacts: '**/*.war'
                    }
                }

            }

            stage('Test'){
                steps{
                sh 'mvn -s settings.xml test'
                }
            }

            stage('Checkstyle Analysis'){
                steps{
                    sh 'mvn -s settings.xml checkstyle:checkstyle'
                }
            }

            stage('Sonar Analysis') {
            environment {
                scannerHome = tool "${SONARSCANNER}"
            }
            steps {
               withSonarQubeEnv("${SONARSERVER}") {
                   sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
              }
            }
        }

       
    }

       post {
        always {
            echo 'Slack Notifications.'
            slackSend channel: '#jenkinscicd',
                color: COLOR_MAP[currentBuild.currentResult],
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
        }
        }
    
}

