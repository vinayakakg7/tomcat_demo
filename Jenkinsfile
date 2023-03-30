
def attachBuildLogs() {
    try {
        def buildLogFile = "${env.BUILD_LOG_MULTIFILE}"
        def logFile = new File(buildLogFile)
        if (logFile.exists()) {
            def attachment = new hudson.model.Attachment(buildLogFile, "text/plain")
            currentBuild.rawBuild.getAttachments().put(attachment)
        }
    } catch (Exception e) {
        println "Failed to attach build logs: ${e}"
    }
}
import hudson.model.Attachment
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
//    stage('Run SonarQube analysis') {
//          steps {

//             script{
//                withSonarQubeEnv(credentialsId: 'sonarapi') {
//                    bat 'mvn clean package sonar:sonar'
//					}
//				}
//			}
//		}
       
//    stage('Check quality gate status') {
//        steps {
//            script {
//                def qg = waitForQualityGate()
//                if (qg.status != 'OK') {
//                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
//                   }
//               }
//            }
//		}
//     stage("deploy-dev"){
//     steps{

//          sshagent(['deploy_User']) { 
        
//						sh  "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/demo/springbootApp.jar ec2-user@15.207.113.178: /opt/tomcat/webapps/ "
//						sh  "ssh -o StrictHostKeyChecking=no ec2-user@15.207.113.178 /opt/tomcat/bin/shutdown.sh"
//						sh  "ssh -o StrictHostKeyChecking=no ec2-user@15.207.113.178 /opt/tomcat/bin/startup.sh" 

          
//					}
//				}
//			}  
//  } 
}  
 post {
        failure {
            script {
                // attach build failure logs
                attachBuildLogs()
                // send failure email
                mail to: 'vinayakg7@gmail.com, vinayaka.kg@cyqurex.com',
                 subject: "Build failed in ${currentBuild.fullDisplayName}",
                 body: """${env.JOB_NAME} build #${env.BUILD_NUMBER} has failed.
            Please investigate and fix the issue\n More info at: ${env.BUILD_URL}"""
            }
        }
        success {
            script {
                // attach build success logs
                attachBuildLogs()
                // send success email
                mail to: 'vinayakg7@gmail.com, vinayaka.kg@cyqurex.com',
                 subject: "Build successful in ${currentBuild.fullDisplayName}",
                 body: """${env.JOB_NAME} build #${env.BUILD_NUMBER} has succeeded.
      Congratulations!\n More info at: ${env.BUILD_URL}"""
            }
        }
    }
}
	
