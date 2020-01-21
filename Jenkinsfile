String credentialsId = 'awsCredentials'
try{
	stage('checkout') {
    node {
      cleanWs()
      checkout scm
    }
  }
	stage("init"){
			node{
					withCredentials([[
					$class: 'AmazonWebServicesCredentialsBinding',
					credentialsId: credentialsId,
					accessKeyVariable: 'AWS_ACCESS_KEY_ID',
					secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
					ansiColor('xterm') {
					sh 'terraform init'
				}
			}
		}
	}
	stage("Plan"){
		node{
			withCredentials([[
					$class: 'AmazonWebServicesCredentialsBinding',
					credentialsId: credentialsId,
					accessKeyVariable: 'AWS_ACCESS_KEY_ID',
					secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
					ansiColor('xterm') {
					sh 'terraform plan'
				}
		}
		
	}
	stage("Approval"){
		node{
			slackSend color: '#BADA55', message: 'Please Verfiy the Plan details and take action'
			input 'Approve or Reject?'
		}
	}
	stage("Apply"){
	}
}
catch (org.jenkinsci.plugins.workflow.steps.FlowInterruptedException flowError) {
  currentBuild.result = 'ABORTED'
}
catch (err) {
  currentBuild.result = 'FAILURE'
  throw err
}
finally {
  if (currentBuild.result == 'SUCCESS') {
    currentBuild.result = 'SUCCESS'
  }
}
