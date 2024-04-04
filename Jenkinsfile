pipeline {
     agent any
     tools {
          maven "MAVEN3"
          jdk "OracleJDK8"
     }
     environment {
                    SNAP_REPO = 'vprofile-snapshot'
                    NEXUS_USER = 'admin'
                    NEXUS_PASS = 'Domain@123'
                    RELEASE_REPO = 'vprofile-release'
                    CENTRAL_REPO = 'vpro-maven-central'
                    NEXUSIP = '172.31.83.10'
                    NEXUSPORT = '8081'
                    NEXUS_GRP_REPO = 'vpro-maven-group'
                    NEXUS_LOGIN = 'nexuslogin'
                    SONARSERVER = 'sonarserver'
                    SONARSCANNER = 'sonarscanner'

     }
     stages {
          stage ('build') {
               steps {
                    sh 'mvn -s settings.xml -DskipTests install '
               }
               post {
                    success {
                         echo "Now archiving.."
                         archiveArtifacts artifacts: '**/*.war'
                    }
               }
          }

          stage ('test') {
            steps {
                sh 'mvn test'
            }
          }

          stage ('Checkstyle Analysis') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
          }
          
           
     }
}