pipeline{
    agent any
    stages{
        stage('github validation'){
          steps{
                 git url: 'https://github.com/akshu20791/addressbook-cicd-project'
          }
        }
        stage('compiling the code'){
          steps{
                 sh 'mvn compile'
          }
        }
        stage('testing the code'){
            steps{
                sh 'mvn test'
            }
        }
        stage('Run Sonarqube') {
            environment {
                scannerHome = tool 'SonarQubeScanner';
            }
            steps {
              withSonarQubeEnv(credentialsId: 'sonarproject', installationName: 'SonarQube') {
                sh  """
                ${scannerHome}/bin/sonar-scanner \
                -Dsonar.projectKey=sqp_246e6f6516fd51bab9b6cc5cacc13e67a84a9ce3 \
                -Dsonar.projectName='sonarproject' \
                -Dsonar.sources=src
                """
              }
            }
        }
        stage('qa of the code'){
            steps{
                sh 'mvn pmd:pmd'
            }
        }
        stage('package'){
            steps{
                sh 'mvn package'
            }
        }
        stage("deploy the project on tomcat"){
            steps{
                sh "sudo mv /var/lib/jenkins/workspace/mypipeline/target/addressbook.war /home/ubuntu/apache-tomcat-9.0.98/webapps/"
            }
        }
    }
}
