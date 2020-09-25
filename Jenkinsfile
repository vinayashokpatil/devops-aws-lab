pipeline {

   agent any
   tools {
    maven 'maven'
   }
   stages {

     stage('Build using maven'){
       steps{
          sh 'mvn clean package'
       }
     }

     stage('Publish the artefacts to Nexus repository'){
       steps{
       /*
          script{
             def mavenpom = readMavenPom file: 'pom.xml'
             nexusArtifactUploader artifacts: [[artifactId: 'webapp', classifier: '', file: "target/{mavenpom.artifactId}-${mavenpom.version}.war", type: 'war']], credentialsId: 'nexus3', groupId: 'com.example.maven-project', nexusUrl: '52.14.24.55:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'devops-aws-lab', version: "${mavenpom}.version"
          } */

          nexusArtifactUploader artifacts: [[artifactId: 'webapp', classifier: '', file: 'target/webapp.war', type: 'war']], credentialsId: 'nexus3', groupId: 'com.example.maven-project', nexusUrl: '172.31.9.39:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'devops-aws-lab', version: '1.0.0'

       }
     }

     stage('Run the ansible playbook to deploy the war file on apache tomcat which is present in the target machine'){
       steps{
          sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible_target', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook /opt/playbooks/copywarfilefromnexustocontroller.yaml -i /opt/playbooks/hosts', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
       }
     }




   }

}
