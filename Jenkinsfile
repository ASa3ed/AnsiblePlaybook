pipeline {
  agent {
    label 'UbuntuDevTest'
  }
  stages {
    stage('SCM Checkout') {
      steps {
        git 'https://github.com/ASa3ed/newMVC'
      }
    }

    stage('Build') {
      steps {
        sh '''ls -la
		        dotnet publish
		        '''
        stash 'artifacts'
      }
    }

    stage('Archive') {
      parallel {
        stage('Archive') {
          steps {
            sh '''zip -qr newMVC.zip bin/Debug/net6.0/publish/*
		        '''
          }
        }

        stage('Git') {
          steps {
            git(url: 'fgdsgfh', branch: 'sgdgh', credentialsId: 'dgsg')
          }
        }

      }
    }

    stage('Nexus Upload') {
      steps {
        nexusArtifactUploader(artifacts: [[artifactId: 'newMVC', classifier: '', file: 'newMVC.zip', type: '.zip']], credentialsId: '2dff2813-888c-40e4-b2eb-9166b116331a', groupId: 'newMVC1', nexusUrl: 'localhost:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'HelloWorld', version: '1.0-First')
      }
    }

    stage('Git Ansible Playbook') {
      steps {
        git 'https://github.com/ASa3ed/AnsiblePlaybook'
      }
    }

    stage('Execute Ansible') {
      steps {
        unstash 'artifacts'
        ansiblePlaybook(credentialsId: 'a02984c3-6744-4a93-9af2-c4ba0db1e80f', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory.inv', playbook: 'playbook.yml')
      }
    }

  }
}