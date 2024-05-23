pipeline{
    agent any
    tools{
        jdk 'jdk17'
        maven 'maven3'
    }
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    stages {
        stage('Checkout From Git'){
            steps{
                git branch: 'master', url: 'https://github.com/eyaboubaker/DEVSCOPS2.git'
            }
        }
        stage('mvn compile'){
            steps{
                sh 'mvn clean compile'
            }
        }
        stage('mvn test'){
            steps{
                sh 'mvn test -DskipTests=true'
            }
        }
      stage('Lynis Security Scan') {
            steps {
                // Exécutez le balayage de sécurité Lynis
                sh 'lynis audit system --no-log'
                // Archivez les résultats de Lynis
                archiveArtifacts 'lynis-report.dat'
            }
        }
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=BoardGame \
                    -Dsonar.java.binaries=. \
                    -Dsonar.projectKey=Petclinic '''
                }
            }
        }
    }
}
