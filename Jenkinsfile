pipeline{
    agent any

    tools {
        maven 'maven'
        jdk 'Java'
    }

    environment{
        GIT_REPO = 'https://github.com/vinayakakg7/tomcat_demo.git'
        GIT_BRANCH = 'main'
    }
    stages {
        stage('Clone Git repository') {
            steps {
                git branch: GIT_BRANCH, url: GIT_REPO
            }
        }
    stage('Build and test using Maven') {
            steps {
                sh 'mvn clean install -DskipTests=true'
            }
        }
   // stage('Run SonarQube analysis') {
     //     steps {

       //      script{
         //       withSonarQubeEnv(credentialsId: 'sonarapi') {
            //        bat 'mvn clean package sonar:sonar'
           //    }
          // }
     //      }
     //  }
       
  //  stage('Check quality gate status') {
    //    steps {
      //      script {
        //        def qg = waitForQualityGate()
          //      if (qg.status != 'OK') {
            //            error "Pipeline aborted due to quality gate failure: ${qg.status}"
              //      }
               //}
            //}
   // }
      stage("deploy-dev"){
       steps{
          sshagent(['deploy_User']) {
        
     		sh  "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/demo/target/springbootApp.jar ec2-user@65.1.108.188:/opt/tomcat/webapps/ "
      		sh  "ssh ec2-user@65.1.108.18 /opt/tomcat/bin/shutdown.sh"
     		sh "ssh ec2-user@65.1.108.18 /opt/tomcat/bin/startup.sh" 

          
    }
	}
      }
    }
}
