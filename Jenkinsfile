pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                deleteDir()
                sh 'echo cloning repo'
                sh 'git clone https://github.com/rarvez77/ansible-task.git' 
            }
        }
        
       stage('Terraform Apply') {
    steps {
        script {
            dir('/var/lib/jenkins/workspace/TASK=1/ansible-task') {
                sh 'ls -la' // Print out the contents of the directory
                sh '/bin/terraform init'
                sh '/bin/terraform validate'
                sh '/bin/terraform plan'
                sh '/bin/terraform apply -auto-approve'
            }
        }
    }
}
        
        stage('Ansible Deployment') {
            steps {
                script {
                    sleep 360
                    ansiblePlaybook becomeUser: 'ec2-user', credentialsId: 'amazonlinux', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/ansible-tf/ansible-task/inventory.yaml', playbook: '/var/lib/jenkins/workspace/ansible-tf/ansible-task/amazon-playbook.yml', vaultTmpPath: ''
                    ansiblePlaybook become: true, credentialsId: 'ubuntuuser', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/ansible-tf/ansible-task/inventory.yaml', playbook: '/var/lib/jenkins/workspace/ansible-tf/ansible-task/ubuntu-playbook.yml', vaultTmpPath: ''
                }
            }
        }
    }
}
