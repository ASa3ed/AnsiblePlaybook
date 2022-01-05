pipeline {
    agent {
      label 'UbuntuDevTest'
    }

    stages {
        stage('SCM Checkout') {
            agent {
              label 'UbuntuDevTest'
            }
            steps {
                git 'https://github.com/ASa3ed/HelloWorld.Net'
            }
        }
        stage('Build') {
            steps {
                sh '''ls -la
		        dotnet publish
		        '''
            }
        }
        stage('Archive') {
            steps {
                sh '''zip -r helloWorld.zip bin/Debug/net6.0/publish/*
		        '''
            }
        }
        stage('Nexus Upload') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'HelloWorld', classifier: '', file: 'helloWorld.zip', type: '.zip']], credentialsId: '2dff2813-888c-40e4-b2eb-9166b116331a', groupId: 'HelloWorld', nexusUrl: 'localhost:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'HelloWorld', version: '1.0-First'
            }
        }
        stage('Git Ansible Playbook') {
            steps {
                git 'https://github.com/ASa3ed/AnsiblePlaybook'
            }
        }
        stage('Execute Ansible') {
            steps {
                ansiblePlaybook credentialsId: 'a02984c3-6744-4a93-9af2-c4ba0db1e80f', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory.inv', playbook: 'playbook.yml'
            }
        }
        
    }
}
