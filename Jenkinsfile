pipeline{
	agent any
	stages {
		stage('Create kubernetes cluster') {
			steps {
				withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-user']]) {
					sh '''
						eksctl create cluster \
						--name udacityclusternd \
						--nodegroup-name standard-workers \
						--node-type t2.small \
						--nodes 2 \
						--nodes-min 1 \
						--nodes-max 3 \
						--region us-west-1 \
						--ssh-access \
						--ssh-public-key my-public-key.pub \
						--managed
					'''
				}
			}
		}
		stage('Create conf file cluster') {
			steps {
				withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-user']]) {
					sh '''
						aws eks --region us-west-1 update-kubeconfig --name udacityclusternd
					'''
				}
			}
		}

	}
	post {
        always {
            sh 'echo "One way or the other, task is completed."'
        }
    }
}
