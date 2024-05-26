pipeline {
	agent any
	stages {
		stage('Playbooks Download'){
			steps{
				echo 'Playbooks Download'
				sh 'sudo rm -rf /var/lib/jenkins/workspace/environment-Provisioninging-and-configuration/iac/'
				sh 'sudo rm -rf /home/ec2-user/playbooks/'
				sh 'sudo mkdir /home/ec2-user/playbooks/'
				sh 'git clone https://github.com/juliocezar84/iac.git'
				sh 'sudo mv /var/lib/jenkins/workspace/environment-Provisioninging-and-configuration/iac/* /home/ec2-user/playbooks'
			}
		}
		stage('Playbooks Validation') {
			steps {
				echo 'Playbooks Validation'
				sh 'sudo ls /home/ec2-user/playbooks/ | while read filename; do sudo ansible-playbook /home/ec2-user/playbooks/$filename --syntax-check; done'
			}
		}
		stage('Provisioning and Configuration'){
			parallel{
				stage('Application Server Provisioning and Configuration'){
					steps {
						echo 'Application Server Provisioning and Configuration'
					}
				}
				stage('Database Server Provisioning and Configuration'){
					steps {
						echo 'Application Server Provisioning and Configuration'
					}
				}
			}
		}
	}
}