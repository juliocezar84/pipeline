pipeline{
	agent any
	stages{
		stage('Playbooks Download'){
			steps{
				echo 'Playbooks Download'
				sh 'rm -rf /var/lib/jenkins/workspace/environment-provisioning-and-configuration/iac/'
				sh 'rm -rf /home/jenkins/playbooks/'
				sh 'mkdir /home/jenkins/playbooks/'
				sh 'git clone https://github.com/juliocezar84/iac.git'
				sh 'mv /var/lib/jenkins/workspace/environment-provisioning-and-configuration/iac/* /home/jenkins/playbooks'
			}
		}
		stage('Playbooks Validation'){
			steps{
				echo 'Playbooks Validation'
				sh 'sudo ls /home/jenkins/playbooks/ | while read filename; do sudo ansible-playbook /home/jenkins/playbooks/$filename --syntax-check; done'
			}
		}
		stage('Provisioning and Configuration'){
			parallel{
				stage('Application Server Provisioning and Configuration'){
					stages{
						stage('Application Server Provisioning'){
							steps{
								echo 'Key Pair Creation'
								sh 'sudo ansible-playbook /home/jenkins/playbooks/key-pair-creation.yml'
								echo 'Virtual Private Cloud Creation'
								sh 'sudo ansible-playbook /home/jenkins/playbooks/virtual-private-cloud-creation.yml'
								echo 'Subnet Creation'
								sh 'sudo ansible-playbook /home/jenkins/playbooks/subnet-creation.yml'
								echo 'Security Group Creation'
								//sh 'sudo ansible-playbook /home/jenkins/playbooks/security-group-creation.yml'
								echo 'Security Group Outbound Rules Creation'
								//sh 'sudo ansible-playbook /home/jenkins/playbooks/security-group-outbound-rules-creation.yml'
								echo 'Security Group Inbound Rules Creation'
								//sh 'sudo ansible-playbook /home/jenkins/playbooks/security-group-inbound-rules-creation.yml'
								echo 'EC2 Instance Creation'
								sh 'sudo ansible-playbook /home/jenkins/playbooks/ec2-instance-creation.yml'
							}
						}
            stage('Application Server Connection Test'){
              steps{
                echo 'Connection Test'
                sh "sudo runuser -l ec2-user -c 'ansible application_server -m ping'"
              }
            }
						stage('Application Server Configuration'){
							steps{
								echo 'Apache Instalation'
                sh "sudo runuser -l ec2-user -c 'ansible-playbook /home/jenkins/playbooks/apache-instalation.yml'"
							}
						}
					}
				}
				stage('Database Server Provisioning and Configuration'){
					stages{
						stage('Database Server Provisioning'){
							steps{
								echo 'Database Server Provisioning'
                sh 'sudo ansible-playbook /home/jenkins/playbooks/ec2-instance-creation.yml'
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
    stage('Code Download'){
      steps{
				echo 'Code Download'
				sh 'sudo rm -rf /var/lib/jenkins/workspace/environment-provisioning-and-configuration/code/'
				sh 'sudo rm -rf /home/jenkins/code/'
				sh 'sudo mkdir /home/jenkins/code/'
				sh 'git clone https://github.com/juliocezar84/code.git'
				sh 'sudo mv /var/lib/jenkins/workspace/environment-provisioning-and-configuration/code/* /home/jenkins/code'
      }
    }
    stage('Code Deploy'){
      steps{
				echo 'Code Deploy'
        sh "sudo runuser -l ec2-user -c 'ansible-playbook /home/jenkins/playbooks/code-deploy.yml'"
      }
    }    
	}
}