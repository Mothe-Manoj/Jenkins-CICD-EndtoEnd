pipeline {
    agent any
    tools{
        maven "mymaven"
    }

    stages {
        stage('Code') {
            steps {
                git branch: 'master', url:"https://github.com/devops0014/one.git"
            }
        }
        stage ('CQA'){
            steps{
                withSonarQubeEnv ('sonar') {
                    sh 'mvn clean verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=Java -Dsonar.projectName="Java"'
                }
            }
        }
        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage ('build'){
            steps{
                sh 'mvn clean package'
            }
        }
        
        stage ('nexus') {
            steps{
                nexusArtifactUploader artifacts: [[artifactId: 'myweb', classifier: '', file: 'target/myweb.war', type: 'war']], credentialsId: 'nexus', groupId: 'in.javahome', nexusUrl: '13.206.208.59:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'myrepo', version: '8.8.4'
                }
            }
        
    }
}
