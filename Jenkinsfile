pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/Dnyaneshwar-Raut/FinanceMe', credentialsId: 'github-creds'
                echo 'Checkout completed successfully.'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install'
                echo 'Build completed successfully.'
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t dnyaneshwarraut/finance-me:1.0 .'
                echo 'Docker image build completed successfully.'
            }
        }
        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                    sh 'docker push dnyaneshwarraut/finance-me:1.0'
                }
                echo 'Docker image pushed successfully.'
            }
        }
        stage('Ansible Deploy') {
            steps {
                ansiblePlaybook become: true, credentialsId: 'ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
                echo 'Ansible deployment completed successfully.'
            }
        }
    }
    triggers {
        githubPush()
    }
}

