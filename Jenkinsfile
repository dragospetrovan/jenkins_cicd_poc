node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'jenkins', url: 'git@github.com:dragospetrovan/webapp_cicd.git']]]) 
      mvnHome = tool 'MAVEN3'
   }
   stage('Build') {
      // Run the maven build
      withEnv(["MVN_HOME=$mvnHome"]) {
            sh '"$MVN_HOME/bin/mvn" clean package'
      }
   }
    stage('publish artifact')  {
        archiveArtifacts artifacts: 'target/HelloWorldWebApp.war', onlyIfSuccessful: true
    } 
    stage ('Deploy'){
     
     ansiblePlaybook colorized: true, credentialsId: 'jenkins', disableHostKeyChecking: true, installation: 'Default', inventory: 'ansible/inventory.dev.yaml', playbook: 'ansible/playbooks/deploy.yaml'
        
    }
   
}
