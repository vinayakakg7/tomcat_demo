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
                bat 'mvn clean install -DskipTests=true'
            }
        }
    stage('Run SonarQube analysis') {
          steps {

             script{
                withSonarQubeEnv(credentialsId: 'sonarapi') {
                    bat 'mvn clean package sonar:sonar'
               }
           }
           }
       }
       
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
      stage('Deploy to tomcat server') {
		  steps {
			  script {
					deploy adapters: [tomcat9(path: '', url: 'http://15.207.113.178:8080')], contextPath: null, war: '**/*.jar'
					
					}
				}
		}
    }
	}