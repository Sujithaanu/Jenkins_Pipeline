pipeline{
    agent any

    tools{
        maven 'Maven_3.9.1'
    }
    stages{
        stage('SCM checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Sujithaanu/java-maven-app.git']])
            }
        }
        stage('Maven_Build'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('Sonarqube scan'){
            steps{
                withSonarQubeEnv("Sonarqube") {
                    sh "${tool('Sonar_4.8')}/bin/sonar-scanner \
                    -Dsonar.host.url=http://ec2-3-110-158-108.ap-south-1.compute.amazonaws.com:9000/ \
                    -Dsonar.login=sqp_59794d82a5521a58c16dd36e5bf5e5675b77942f \
                    -Dsonar.projectKey=Maven-Java \
                    -Dsonar.java.binaries=target"
                }
            }
        }
        stage('Nexus-upload'){
            steps{
                sh 'mvn -s settings.xml clean deploy'
            }
        }
    }
}



