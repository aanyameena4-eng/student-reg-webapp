properties([pipelineTriggers([githubPush()])])
node {
    def mavenHome = tool name: 'maven_3.9.12', type: 'maven'

    stage('git') {
        git branch: 'main', 
            credentialsId: 'github', 
            url: 'https://github.com/aanyameena4-eng/student-reg-webapp.git'
    }

    stage('maven build') {
        sh "${mavenHome}/bin/mvn clean package"
    }

    stage("sonar analysis") {
        withCredentials([string(credentialsId: 'stoken', variable: 'sotoken')]) {
        sh "${mavenHome}/bin/mvn clean verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.token=${sotoken}"
        }
    }
        stage('ild') {
        sh "${mavenHome}/bin/mvn clean deploy "
        }
    stage('deploy to tomcat') {
       deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'tomcatid2', path: '', url: 'http://172.31.5.28:8080')], contextPath: null, war: 'target/student-reg-web*war'
    }
}
