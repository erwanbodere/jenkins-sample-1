// Powered by Infostretch 

timestamps {

node () {

	stage ('APP-IC - Checkout') {
 	 checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'git-login', url: 'https://github.com/bnasslahsen/jenkins-sample-1.git']]]) 
	}
	stage ('APP-IC - Build') {
 			// Maven build step
	withMaven(maven: 'maven') { 
 			if(isUnix()) {
 				sh "mvn clean package " 
			} else { 
 				bat "mvn clean package " 
			} 
 		} 
	}
	stage ('APP-IC - Quality Analysis') {
	withMaven(maven: 'maven') { 
 			if(isUnix()) {
 				sh "mvn sonar:sonar" 
			} else { 
 				bat "mvn sonar:sonar" 
			} 
 		} 
       }
	stage ('APP-IC - Deploy') {
	withMaven(maven: 'maven') { 
 			if(isUnix()) {
 				sh "mvn deploy" 
			} else { 
 				bat "mvn deploy" 
			} 
 		} 
	}
	
	
	stage("Release") {
            when {
                expression { params.RELEASE }
            }
            steps {
                sh "mvn -B release:prepare"
                sh "mvn -B release:perform"
            }
        }
	
     parameters {
        booleanParam(name: "RELEASE",
                description: "Build a release from current commit.",
                defaultValue: false)
    }
	
	stage ('APP-IC - Post build actions') {
/*
Please note this is a direct conversion of post-build actions. 
It may not necessarily work/behave in the same way as post-build actions work.
A logic review is suggested.
*/
		// Mailer notification
		step([$class: 'Mailer', notifyEveryUnstableBuild: true, recipients: 'erwan.bodere@gmail.com', sendToIndividuals: false])
 
	}
}
}
