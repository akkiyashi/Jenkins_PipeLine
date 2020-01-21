pipeline{
		agent any
		stages{
			stage("Init"){
				steps{
					sh 'terraform init'
				}
			}
			stage("Plan"){
				steps{
					sh 'terraform plan'
				}
			}
			stage("Approval"){
				steps{
					slackSend color: '#BADA55', message: 'Please Verfiy the Plan details and take action'
					input 'Approve or Reject?'
				}
			}
			stage("Apply"){
				steps{
					sh 'terraform apply --auto-approve'
				}
			}
		}
}
