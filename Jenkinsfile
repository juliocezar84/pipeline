pipeline{
	agent any
	stages{
		stage('Playbooks Download'){
			steps{
				echo 'Playbooks Download'
				sh 'sudo rm -rf /var/lib/jenkins/workspace/environment-provisioning-and-configuration/iac/'
				sh 'sudo rm -rf /home/ec2-user/playbooks/'
				sh 'sudo mkdir /home/ec2-user/playbooks/'
				sh 'git clone https://github.com/juliocezar84/iac.git'
				sh 'sudo mv /var/lib/jenkins/workspace/environment-provisioning-and-configuration/iac/* /home/ec2-user/playbooks'
			}
		}
		stage('Playbooks Validation'){
			steps{
				echo 'Playbooks Validation'
				sh 'sudo ls /home/ec2-user/playbooks/ | while read filename; do sudo ansible-playbook /home/ec2-user/playbooks/$filename --syntax-check; done'
			}
		}
		stage('Provisioning and Configuration'){
			parallel{
				stage('Application Server Provisioning and Configuration'){
					stages{
						stage('Application Server Provisioning'){
							steps{
								echo 'Key Pair Creation'
								sh 'sudo ansible-playbook /home/ec2-user/playbooks/key-pair-creation.yml'
								echo 'Virtual Private Cloud Creation'
								sh 'sudo ansible-playbook /home/ec2-user/playbooks/virtual-private-cloud-creation.yml'
								echo 'Security Group Creation'
								//sh 'sudo ansible-playbook /home/ec2-user/playbooks/security-group-creation.yml'
								echo 'Security Group Outbound Rules Creation'
								//sh 'sudo ansible-playbook /home/ec2-user/playbooks/security-group-outbound-rules-creation.yml'
								echo 'Security Group Inbound Rules Creation'
								//sh 'sudo ansible-playbook /home/ec2-user/playbooks/security-group-inbound-rules-creation.yml'
								echo 'EC2 Instance Creation'
								sh 'sudo ansible-playbook /home/ec2-user/playbooks/ec2-instance-creation.yml'
							}
						}
            stage('Application Server Connection Test'){
              steps{
                echo 'Connection Test'
								sh 'whoami'
              }
            }
						stage('Application Server Configuration'){
							steps{
								echo 'Apache Instalation'
								//sh 'sudo ansible-playbook /home/ec2-user/playbooks/apache-instalation.yml'
							}
						}
            stage('Code Deploy'){
              steps{
								echo 'Code Download'
								//sh 'sudo ansible-playbook /home/ec2-user/playbooks/apache-instalation.yml'
                echo 'Code Deploy'
								//sh 'sudo ansible-playbook /home/ec2-user/playbooks/apache-instalation.yml'
							}
            }
					}
				}
				stage('Database Server Provisioning and Configuration'){
					stages{
						stage('Database Server Provisioning'){
							steps{
								echo 'Database Server Provisioning'
							}
						}
						stage('Database Server Configuration'){
							steps{
								echo 'Database Server Configuration'
							}
						}
					}
				}
			}
		}
	}
}