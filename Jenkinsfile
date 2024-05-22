pipeline {
	agent any
	stages {
		stage('Playbooks Download'){
			steps{
				echo 'Playbooks Download'
				sh 'sudo rm -rf /var/lib/jenkins/workspace/environment-provisioning-and-configuration/iac/'
				//sh 'sudo rm -rf /home/ec2-user/playbooks/'
				//sh 'mkdir /home/ec2-user/playbooks'
				sh 'git clone https://github.com/juliocezar84/iac.git'
				sh 'sudo mv /var/lib/jenkins/workspace/environment-provisioning-and-configuration/iac/* /home/ec2-user/playbooks'
			}
		}
		stage('Playbooks Validation') {
			steps {
				echo 'Playbooks Validation'
				sh 'sudo ls /home/ec2-user/playbooks/ | while read filename; do sudo ansible-playbook /home/ec2-user/playbooks/$filename --syntax-check; done'
			}
		}
		stage('Key Creation') {
			steps {
				echo 'Key Creation'
				sh 'sudo ansible-playbook /home/ec2-user/playbooks/key-pair-creation.yml'
			}
		}
		stage('Virtual Private Cloud Creation') {
			steps {
				echo 'Virtual Private Cloud Creation'
				sh 'sudo ansible-playbook /home/ec2-user/playbooks/virtual-private-cloud-creation.yml'
			}
		}
		stage('EC2 Instance Creation') {
			steps {
			echo 'EC2 Instance Creation'
			sh 'sudo ansible-playbook /home/ec2-user/playbooks/ec2-instance-creation.yml'
			}
		}
	}
}