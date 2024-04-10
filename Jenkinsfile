pipeline {
    agent any 
    environment {
                    SNAP_REPO = 'vprofile-snapshot'
                    NEXUS_USER = 'admin'
                    NEXUS_PASS = 'Domain@123'
                    RELEASE_REPO = 'vprofile-release'
                    CENTRAL_REPO = 'vpro-maven-central'
                    NEXUSIP = '172.31.19.174'
                    NEXUSPORT = '8081'
                    NEXUS_GRP_REPO = 'vpro-maven-group'
                    NEXUS_LOGIN = 'nexuslogin'
                    SONARSERVER = 'sonarserver'
                    SONARSCANNER = 'sonarscanner'

    }
    stages {
        stage ('build') {
            steps {
                sh 'mvn -DskipTests install'
            }
            post {
                success {
                    echo "Now archiving the artifact built....."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage ('test') {
            steps {
                sh 'mvn test'
            }
        }
        stage ('checkstyle analysis') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage('Sonarqube') {
          environment {
             scannerHome = tool "${SONARSCANNER}"
           }
           steps {
              withSonarQubeEnv("${SONARSERVER}") {
                sh "${scannerHome}/bin/sonar-scanner"
              }
              timeout(time: 10, unit: 'MINUTES') {
                  waitForQualityGate abortPipeline: true
              }
           }      
        }

    }
}